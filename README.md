# Supabase RLS rules for AI coding agents

Rules that make Cursor, Claude Code, Codex and other agents write Supabase migrations, Row Level Security policies and grants that don't leak data.

AI tools reach for the fixes that make an error go away: `grant ... on all tables`, `using (true)`, turning RLS off, or putting the `service_role` key in the frontend. These rules steer them to the version that works **and** stays private: RLS in the same migration as the table, one policy per operation, per-table grants, security definer helpers in a private schema, and tests as real roles.

They also map the common errors to their causes: `42501 permission denied for table`, `42P17 infinite recursion detected in policy`, `PGRST116`, `PGRST205`, and an update/delete that silently changes nothing.

## Install

**Cursor**: copy [`.cursor/rules/supabase-rls.mdc`](.cursor/rules/supabase-rls.mdc) into your project's `.cursor/rules/`.

**Claude Code** (plugin):

```
/plugin marketplace add mateuszingano/supabase-rls-rules
/plugin install supabase-rls@supabase-rls-rules
```

Or copy [`skills/supabase-rls/`](skills/supabase-rls/) into `.claude/skills/` in your project.

**Codex, Copilot, Gemini and others**: copy the content of [`AGENTS.md`](AGENTS.md) into your project's `AGENTS.md` (or the instructions file your agent reads).

## Back the rules with a gate

Rules make the agent write better SQL; they don't prove it. [`airlock-rls`](https://github.com/mateuszingano/airlock-rls) fails your CI build when a table ships without RLS or with an always-true policy:

```
npx airlock-rls
```

[`airlock-rls-mcp`](https://github.com/mateuszingano/airlock-rls-mcp) lets the agent run the same check (and a scan for an exposed `service_role` key) while it builds.

## Why each rule

Every rule links back to a guide with SQL tested against Postgres: [shipsealed.com/rls](https://shipsealed.com/rls/?utm_source=github&utm_medium=referral&utm_campaign=supabase-rls-rules).

## License

MIT
