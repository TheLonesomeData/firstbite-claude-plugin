# First Bite

First Bite is the go-to-market platform for foodservice: operator data,
prospecting and CRM in one place. This plugin connects Claude to your First
Bite account, so Claude works on the same data you see in the app:

- **Operator data and prospecting:** find commercial and non-commercial
  operators and their locations, with First Bite's filters and menu search.
- **CRM:** your accounts, contacts, deals, samples, activities and reminders,
  and reporting on your pipeline.
- **Playbooks:** step-by-step jobs written by First Bite's customer success
  team, for filing call notes, following up on a call, and creating a deal.

## Install

In Claude Code:

    /plugin install firstbite --marketplace TheLonesomeData/firstbite-claude-plugin

On claude.ai and in Cowork, your Claude organization's owner adds the plugin
under Organization settings > Plugins & skills. Then open the plugin's
Connectors tab and sign in to First Bite.

You need a First Bite account with assistant access. Ask your First Bite admin
or write to support@firstbite.io.

## What it connects to

The connector is First Bite's MCP server at
`https://api.firstbite.io/api/v1/mcp/`. You sign in with your First Bite login,
and Claude only sees what your account can see in the app.

Privacy policy: https://app.firstbite.io/privacy-policy
Terms of use: https://app.firstbite.io/terms-of-use
