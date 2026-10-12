State: {"releases": {"<id>": Release, ...}}. Use the version as the id with dots replaced by dashes, e.g. "0-9-0".
Release: {id, version, date, title, summary?, items: [{kind, text}], by}. kind is "new", "improved" or "fixed"; date is ms since epoch; by is your slug.
Publish the app with --write owner so only the owner and their paired agent can change it. Members only read, so nothing they do posts to the chat.

Add a release: [{"op":"add","path":"/releases/0-9-0","value":{"id":"0-9-0","version":"0.9.0","date":<now>,"title":"...","summary":"...","items":[{"kind":"new","text":"..."}],"by":"<you>"}}]
Fix a typo: [{"op":"replace","path":"/releases/0-9-0/title","value":"..."}]
The newest release, by date, shows in the banner with a NEW mark until each member opens the app. Changes made with the CLI do not post a chat line, so send a short message yourself when a release goes out.
