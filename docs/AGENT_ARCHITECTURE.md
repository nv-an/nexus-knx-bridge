# Agent Architecture and File Roles

NEXUS KNX Bridge uses an AI agent as a product feature and also includes
optional instructions for development tools. These concerns are intentionally
separated. The presence of an agent-related file describes runtime behavior or
the development workflow; repository authorship is represented by Git commit
metadata.

## OpenClaw product runtime

The following files are part of the deployed smart-home agent:

- `openclaw/workspace-template/AGENTS.md` defines KNX request routing, safety,
  authorization, and tool behavior.
- `openclaw/workspace-template/IDENTITY.md`, `SOUL.md`, and `TOOLS.md` define the
  assistant identity and available product tools.
- `skills/` contains the OpenClaw skills used for KNX control, ETS processing,
  scenes, memory, documents, and approved skill lifecycle operations.
- `services/openclaw_config_service.py` bootstraps the workspace template and
  maintains the runtime link to the canonical `skills/` directory.

The workspace bootstrap code loads only `AGENTS.md`, `IDENTITY.md`, `SOUL.md`,
and `TOOLS.md` from `openclaw/workspace-template/`. Product permissions remain
subject to OpenClaw Gateway identity, allow-list, confirmation, and skill
policy checks.

## Development tooling

The following files are used only while maintaining the repository:

- `.agents/AGENTS.md` contains general repository-maintenance rules for
  automated development tools.
- `frontend/CLAUDE.md` is a development-tool entry point that imports the
  frontend compatibility notes.
- `frontend/AGENTS.md` contains version-specific guidance for contributors and
  development tools working on the Next.js frontend.
- `docs/DECISIONS.md` is engineering reference material for maintainers. The
  running application does not load it automatically.

These files are not imported into the Python service, bundled by Next.js, or
copied into the OpenClaw workspace by the installer.

## Authorization boundary

OpenClaw runtime actions and repository-development automation are distinct.
An OpenClaw skill may change system state only through its configured tool and
authorization path. Development-tool instructions do not grant runtime access,
do not replace application-layer authorization, and do not authorize a tool to
modify or deploy code without the required owner approval.
