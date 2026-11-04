## Plan: Add FastAPI Backend Tests

Add a focused pytest suite under `tests/` for the FastAPI endpoints, using `TestClient` and isolated in-memory activity data so tests are repeatable and do not depend on execution order. Structure every test with clearly separated Arrange, Act, and Assert phases. Keep pytest out of runtime dependencies by declaring it in `requirements-dev.txt`.

**Steps**
1. Add `requirements-dev.txt` that includes `-r requirements.txt` and `pytest`; reuse the existing `httpx` dependency for FastAPI's `TestClient`.
2. Add `tests/test_app.py` with a `TestClient` fixture. Use pytest's `monkeypatch` plus `deepcopy` of the seeded activity dictionary to replace the module-level `activities` per test and restore the original after each test.
3. Write each test in AAA order: Arrange any request data and expected state, Act with one client request, then Assert the response and resulting activity state. Cover `GET /activities`, successful signup and participant update, signup for an unknown activity (404), duplicate signup (400), successful removal and participant update, removal from an unknown activity (404), and removal of an email not registered (404).
4. Add a concise test-install/run section to `src/README.md` using `python -m pip install -r requirements-dev.txt` and `python -m pytest`.

**Relevant files**
- `/workspaces/skills-getting-started-with-github-copilot/src/app.py` — current routes and module-level `activities` map; tests exercise these without changing application behavior.
- `/workspaces/skills-getting-started-with-github-copilot/pytest.ini` — existing `pythonpath = .` supports importing the app module from the test suite.
- `/workspaces/skills-getting-started-with-github-copilot/requirements.txt` — already includes `httpx`; leave runtime dependencies unchanged.
- `/workspaces/skills-getting-started-with-github-copilot/requirements-dev.txt` — add shared runtime requirements plus pytest.
- `/workspaces/skills-getting-started-with-github-copilot/tests/test_app.py` — new backend test module, per-test isolation fixture, and AAA-structured tests.
- `/workspaces/skills-getting-started-with-github-copilot/src/README.md` — document how to install development dependencies and run tests.

**Verification**
1. Install test dependencies with `python -m pip install -r requirements-dev.txt`.
2. Run the new suite with `python -m pytest` from the repository root.
3. Confirm each mutation test starts from seeded state and does not affect a subsequent test; check each test follows Arrange, Act, Assert order.

**Decisions**
- Use pytest with FastAPI's `TestClient`; no new test framework or async client is needed because all current routes are synchronous.
- Keep test dependencies in `requirements-dev.txt`, not runtime `requirements.txt`.
- Use explicit AAA sections in individual tests to make setup, request action, and expectations immediately scannable.
- Do not refactor routes, add database persistence, add frontend tests, or broaden the scope beyond the existing API contracts.