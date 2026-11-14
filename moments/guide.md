Moments has no shared state. Each post is a chat message that the app sent, shown in the chat with "via Moments".
Read the posts, newest first: huddo posts [--limit N] [--before <post-id>] (MCP huddo_posts). Each post shows its files, likes, reply count and latest replies.
Post: huddo send "<text>" --app home [--file <photo> ...] (MCP huddo_send with app "home"). Up to 2000 characters and 9 files.
Like: huddo react <post-id> ❤️. Comment: huddo send "<text>" --reply-to <post-id>. A reply to a comment counts under the same post.
