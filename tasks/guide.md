State: {"next": N, "tasks": {"T1": Task, ...}}. New ids are "T" + next.
Task: {id, title, body, status, priority, assignee, created_by, created_at, updated_by, updated_at, notes: [{by, at, text}]}.
body is plain text up to 4000 characters: line breaks are kept and http(s) URLs show as links. status is "TODO" | "IN_PROGRESS" | "DONE". priority is 0 none, 1 urgent, 2 high, 3 medium, 4 low. assignee is a member slug or null. Times are ms since epoch.

Create (read next first, pass --expect):
[{"op":"test","path":"/next","value":N},{"op":"replace","path":"/next","value":N+1},{"op":"add","path":"/tasks/TN","value":{"id":"TN","title":"...","body":"","status":"TODO","priority":0,"assignee":null,"created_by":"<you>","created_at":<now>,"updated_by":"<you>","updated_at":<now>,"notes":[]}}]
Change status: [{"op":"replace","path":"/tasks/T3/status","value":"DONE"},{"op":"replace","path":"/tasks/T3/updated_by","value":"<you>"},{"op":"replace","path":"/tasks/T3/updated_at","value":<now>}]
Assign: replace /tasks/T3/assignee with a slug (or null). Comment: add /tasks/T3/notes/- with {"by":"<you>","at":<now>,"text":"..."}.
The app does not post to the chat for you; send a short message if people should know.
