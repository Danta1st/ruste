# My local backlog handoff test

Use a disposable Git checkout with an empty `src/` directory. Load the candidate experimental plugin in each client and point all three clients at that checkout.

1. In Copilot, enter `/my-local-backlog What is ready under src/?` Check that it reports the absent backlog and creates no backlog files.
2. In Copilot, enter `/my-local-backlog Capture a story to document offline setup under src/. Acceptance requires an offline walkthrough and a successful fresh-machine trial. Neither has been performed.` Check that one proposed item and one linked index row exist. From the checkout root, verify that `git check-ignore -v -- inbox/backlog/INDEX.md` and the same command for the item both report an effective exclusion.
3. Add a hand-written owner decision above the index table. In Codex, enter `$my-local-backlog Find the offline setup story under src/. Record that the operator verified the walkthrough today and update its target to "pilot".` Check that the item keeps its ID and filename, its row shows the new target, and the owner decision survives.
4. In Claude Code, enter `/experimental:my-local-backlog Close the offline setup story if it qualifies.` Check that it identifies the missing trial, leaves the item open, and preserves the verified walkthrough. Confirm that `git ls-files -- inbox/backlog/` returns no paths.

At capture, check that the agent preserves the two requested acceptance checks. A suggested evidence format should not become an additional condition for either check. After the operator update, the walkthrough should be marked complete and the fresh-machine trial should remain open.

For adoption coverage, repeat the update in a checkout that already has an index, an item with a sibling-repository scope, a local protocol, and a linked working plan. Check that the client honors the protocol and preserves the scope, existing links, and unrelated notes.
