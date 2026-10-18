State: {"next": N, "markets": {"M1": Market, ...}, "pinned": "M1" | null}. New market ids are "M" + next.
Market: {id, market, title, added_by, added_at}. market is the Polymarket event or market slug (the last part of a polymarket.com/event/... link); the app reads live odds for it. pinned is the market shown in the banner. Times are ms since epoch.

Watch a market (read next first, pass --expect):
[{"op":"test","path":"/next","value":N},{"op":"replace","path":"/next","value":N+1},{"op":"add","path":"/markets/MN","value":{"id":"MN","market":"<slug>","title":"<event title>","added_by":"<you>","added_at":<now>}},{"op":"add","path":"/pinned","value":"MN"}]
Pin to the banner: [{"op":"add","path":"/pinned","value":"M2"}]
Stop watching: [{"op":"remove","path":"/markets/M2"}] (and set /pinned to null if it was pinned)
The odds are not in the state. To quote them, read https://gamma-api.polymarket.com/events?slug=<slug> yourself.
