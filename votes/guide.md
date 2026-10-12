State: {"next": N, "polls": {"P1": Poll, ...}}. New poll ids are "P" + next.
Poll: {id, question, options: [{id: "o1", label}, ...], multi, closed, created_by, created_at, closed_at?, votes: {"<member slug>": ["o1", ...]}}.
multi false means a ballot holds at most one option id. An empty list means the member withdrew their vote. Times are ms since epoch.

Create a poll (read next first, pass --expect):
[{"op":"test","path":"/next","value":N},{"op":"add","path":"/next","value":N+1},{"op":"add","path":"/polls/PN","value":{"id":"PN","question":"...","options":[{"id":"o1","label":"..."},{"id":"o2","label":"..."}],"multi":false,"closed":false,"created_by":"<you>","created_at":<now>,"votes":{}}}]
Vote (replaces your ballot): [{"op":"add","path":"/polls/P2/votes/<you>","value":["o1"]}]
Close (by convention only the creator closes): [{"op":"add","path":"/polls/P2/closed","value":true},{"op":"add","path":"/polls/P2/closed_at","value":<now>}]
Count results from votes. Don't vote for other members, and don't change a closed poll.
