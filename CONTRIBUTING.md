# Contributing

Changes reach `main` only through pull requests. Direct pushes are blocked, and every pull request needs an approving review from a maintainer.

1. Fork the repo and create a branch.
2. Add or change one app per pull request. Each app lives in its own folder with `app.html`, `state.json`, `guide.md`, `README.md` and, ideally, a `screenshot.png`.
3. Before opening the pull request, test the app in a real huddo, ideally your own. Publish it with `huddo app publish <app-id> --file app.html ... --room <space-id>`. Check both the panel and the banner (if it has one), in light and dark themes.
4. Keep apps self-contained. They run sandboxed: no external scripts, styles, fonts, images or network calls.
5. Escape anything from the shared state before putting it into HTML.
6. Open the pull request with a short description and a screenshot.
