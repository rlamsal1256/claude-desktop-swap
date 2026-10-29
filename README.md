# claude-desktop-swap

`cdswap` switches the Claude Desktop app on macOS between saved accounts, and keeps one shared list of Code sessions across them.

## Install

```sh
ln -s "$PWD/cdswap" ~/.local/bin/cdswap
```

## Use

```sh
cdswap new           # clear the local login so Desktop shows a fresh sign-in
cdswap add <name>    # save the signed-in account as <name>
cdswap use <name>    # switch to a saved account
cdswap list          # * marks the active account
```

To add an account, run `cdswap new`, sign in, then `cdswap add <name>`.

Each `add` and `use` quits and relaunches Desktop.

**Never sign out in Desktop.** Signing out revokes the token on the server, and the saved profile for that account stops working. If that happens, sign the account in again with `cdswap new` and `cdswap add <name>`.

## What it changes

A profile in `~/.claude-desktop-swap/<name>/` holds the account's login:

- `oauth:tokenCacheV2` and `lastKnownAccountUuid` from Desktop's `config.json`
- the `Cookies` file
- the `Local Storage` folder

Desktop stores Code sessions in `claude-code-sessions/<account>/<org>/`. The session files hold no account ID, so `add` replaces the new account's org folder with a symlink to the existing one. All accounts then see the same sessions and scheduled tasks. Cowork sessions are not shared.
