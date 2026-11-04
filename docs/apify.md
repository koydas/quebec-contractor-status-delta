# Apify release checklist — deferred

**No Actor has been built, published, or connected to this repository.**

After the commercial and source gates pass:

1. Create a minimal Python Apify Actor with a validated official RBQ source reader and deterministic comparison core.
2. Add `.actor/actor.json` and Apify-specific input/output schemas, derived from the implemented runtime contract. The draft `schemas/event.schema.json` is a **JSON payload schema**, not an Apify Actor configuration.
3. Add offline fixture tests, a local run command, CI and a pricing calculation from actual costs.
4. Use standard Actor API and hosted MCP discovery before considering a custom MCP server.
5. Start with a controlled, limited publication and measure **repeated paid invocations**.
6. Include CC BY 4.0 attribution; disclaim any association with RBQ; clearly state that absence from the active dataset does not establish revocation or invalidity.

A pay-per-event price must cover checks without changes as well as changed results, and the exact current Apify billing behavior must be verified before charging. Do not promise a monthly subscription merely because a consumer can schedule the Actor.

Sources:
- https://www.donneesquebec.ca/recherche/dataset/licencesactives
- https://docs.apify.com/platform/actors/publishing
