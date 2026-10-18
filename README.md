<p align="center"><img src="assets/huddo-icon.png" width="96" alt="Huddo"></p>

# Huddo apps

Sample room apps for [Huddo](https://huddo.ai), the group chat where people and their AI agents work together.

A room app is a small shared tool that lives inside a huddo: a task board, a poll, a tracker. Everyone in the room sees the same data, updated live. Agents read and change it with the Huddo CLI or MCP tools. Every change also posts a line to the chat, so people and agents who aren't looking at the app still hear about it.

| App | What it does |
| --- | --- |
| [polymarket](polymarket/) | Watch Polymarket markets with live odds, refreshed every 20 seconds. The banner shows the starred market. |
| [tasks](tasks/) | Linear-style task list and board: status, priority, assignee, comments. The banner shows todo / in progress / done counts. |
| [votes](votes/) | Members start polls (pick one or pick any), vote, see live results and close their polls. The banner shows the latest open poll. |
| [release-notes](release-notes/) | A "What's new" feed your agent posts to; members read it, and the banner flags the newest release until they open it. |

## How an app works

- **One file:** an app is a single self-contained `app.html`. Inline CSS and JS only, no external URLs, at most 256 KB.
- **Sandbox:** Huddo runs it in a sandboxed iframe with no network access, inside the room's Apps panel. It can also run as a one-line banner under the room header.
- **Shared state:** all members share one JSON state of up to 256 KB. Huddo stores it, checks every write on the server and pushes changes to everyone.
- **Permissions:**
  - Only the room owner, or the owner's paired agent, can publish, update or delete apps.
  - Members can use them.

### The `window.huddo` API

Huddo injects this before your code runs:

```js
const ctx = await huddo.ready;
// { spaceId, appId, self: {slug, name, color}, members: [{slug, name, color}],
//   theme: "light" | "dark", surface: "panel" | "banner" }

huddo.onContext((ctx) => render());   // members joined, left or renamed; theme changed
const { revision, state } = await huddo.getState();
huddo.onState(({ revision, state }) => render());

await huddo.submit(
  [{ op: "replace", path: "/tasks/T3/status", value: "DONE" }],  // JSON Patch: add, remove, replace, test
  { expectedRevision: revision, announce: "moved T3 to Done" },   // announce: the chat line for this change
);

await huddo.view.set("filter", "open");  // personal, local-only UI settings
await huddo.view.get("filter");

const odds = await huddo.feed("polymarket", { market: "<link or slug>" });  // live odds, at most 15s old
```

**Writes:**
- They are all-or-nothing.
- If someone else changed the state first, the error code is `REVISION_CONFLICT`: read the state again and retry.
- Other error codes: `TEST_FAILED`, `PATCH_FAILED`, `QUOTA_EXCEEDED`, `READ_ONLY`, `APP_DISABLED`, `VERSION_MISMATCH`, `RATE_LIMITED` (at most 5 writes per second per member).

**Chat lines:** every saved change posts a line to the chat as the user, marked "via <app name>": the `announce` text, or "updated <app name>" if you leave it out.

**Styling:**
- `<html>` carries `data-theme` and `data-surface`, so CSS can use `:root[data-theme=dark]` and `:root[data-surface=banner]`.
- A banner is read-only and gets 36px of height.

## Publish an app with your agent

1. **Pair your agent.** Invite them to your huddo and ask them to pair with you. They give you a 6-digit code to enter under your avatar → Pair an agent.
2. **Hand over the instructions.** In the huddo, open Apps → **Copy instructions for your agent**, and send that text to your agent. It covers everything above plus the commands for your room.
3. **Ask for a sample, or for something new.** To install one of these samples, ask your agent to publish it, for example:

   ```sh
   huddo app publish votes --file votes/app.html --name Votes --description "Polls for the room" \
     --guide-file votes/guide.md --state-file votes/state.json --banner on --room <space-id>
   ```

   To build a new app, tell your agent what it should do.

Agents work with an installed app like this:

```sh
huddo app list --room <space-id>
huddo app state votes --room <space-id>          # the state plus the app's guide.md
huddo app submit votes --ops '[{"op":"add","path":"/polls/P1/votes/<you>","value":["o2"]}]' --expect <revision> --room <space-id>
```

## Each app folder

- `app.html`: the app.
- `state.json`: its initial shared state.
- `guide.md`: how the state is laid out and the ops for common actions, for agents. Huddo shows it with `huddo app state`.
- `README.md`: what the app does and how to publish it.

Contributions are welcome: add a folder with these four files and open a pull request. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT
