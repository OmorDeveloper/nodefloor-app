# How it works

## The floor

The office view is not a metaphor laid over a list. Every desk is a **node** —
an agent — and every node is a real terminal process on your machine running a
coding CLI. When a node is working you see it at its desk; when it is waiting on
you, it says so above its head.

The point of drawing it this way is that several agents working at once is hard
to follow as a list of log lines and easy to follow as a room.

![The floor and the dashboard](../media/app-dashboard.png)

## Core

**Core is the orchestrator**, and the boss. He runs the floor: takes what you
ask for, decides which node should do it, hands the work over and reports back.
You can talk to any node directly, but talking to Core is the normal way to use
NodeFloor.

Core occupies a desk and spends tokens like any other node, which is why **he
counts against your node limit**. The free tier's three nodes are Core plus two.

## The command centre

The panel on the right is whichever tab you are on. The tabs run down the side
in one column — there are nineteen of them, and across the top they wrapped onto
four rows and spent a sixth of the panel on navigation.

| Tab | What it is for |
|---|---|
| **terminal** | The selected node's actual terminal. Type into it. |
| **dashboard** | Who is on the floor, who is working, who is blocked. |
| **monitor** | The floor at a glance while something long is running. |
| **tasks** | A kanban board the nodes read and write. |
| **ask me** | Everything waiting on a human decision, in one place. |
| **triggers** | Schedules and webhooks. |
| **memory** | What each node remembers, and editing it. |
| **sharing** | Rules for passing memory between nodes. |
| **graph** | Memory as a graph rather than a list. |
| **extract** | Scrape a page or a table into CSV. |
| **hermes** | The bundled Hermes runtime. |
| **activity** | What happened, in order. |
| **skills** | Installed skills; edit, add or remove. |
| **nodes** | Hire, edit and retire nodes. |
| **crm** | A local contact database the nodes can work in. |
| **posts** | Scheduled posts to your own accounts. |
| **flows** | The workflow canvas. |

## Memory

Each node has its own memory, in plain markdown files inside your home folder.
You can read and edit them by hand; nothing is in a format you need this app to
open.

**Sharing** is explicit. A node does not see another node's memory unless you
write a rule saying so. Rules are checked for the obvious mistakes — sharing
with yourself, duplicates, loops — because a cycle would otherwise copy the same
note back and forth forever.

## Workflows

The flows tab is a canvas of nodes: triggers, HTTP calls, conditions, email,
posts, file delivery, agent hand-offs. Every node type on the palette is one the
engine can actually run — there is nothing in the picker that fails at three in
the morning.

![Workflows and n8n import](../media/app-workflows.png)

**Templates arrive switched off** and list what they still need from you. A
workflow that runs the moment you click it, before you have filled in the
account it posts to, is a worse default than one that waits.

### Importing from n8n

Paste or open an n8n workflow export and it converts: nodes, connections,
branches and cron expressions. Sticky notes are dropped — measured across 2,077
real workflows, they appeared 5,791 times and were connected to something zero
times.

## Hermes

Hermes ships **inside the app, with its own Python**. There is no runtime to
install and no virtualenv to activate. The Hermes tab exposes the parts of its
command line worth a button — status, install check, tools, skills — and the
rest of the command line is still there if you want it.

![Hermes](../media/app-hermes.png)

## Sending things out

Reports, PDFs and spreadsheets can go straight to a person:

- **Email**, through your own SMTP server. Not a sending service — your account.
- **Telegram**, through your own bot.
- **WhatsApp**, through the WhatsApp Business Cloud API. This one has real
  restrictions: it needs a Meta Business account, and a free-form message may
  only be sent inside the 24-hour window opened by the recipient's last message
  to you. Outside that window the API accepts the message and never delivers it.

Email and Telegram have neither restriction, which is why they come first.

## Where your data is

In your home folder, on your disk. There is no account, no sync and no server of
ours in the path. Prompts, code and credentials do not leave your machine except
when you explicitly send something out — an email, a post, a webhook — and then
they go straight to that service under your own credentials.

Secrets — API keys, SMTP passwords, bot tokens — are held encrypted and read
only by the main process at the moment they are used. They never cross into the
UI and never reach a log.
