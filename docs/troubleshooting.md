# Troubleshooting

## Windows says the publisher is unknown

Expected. The installer is not code-signed. *More info* → *Run anyway*, and
check the SHA-256 against the one on the release if you want to be careful. See
[install.md](install.md).

## No agent CLI was found

NodeFloor drives a coding CLI rather than talking to a model itself, so it needs
one on your `PATH`. Open a new terminal and check the one you expect actually
runs there — `claude --version`, say. If it works in one terminal and not
another, the installer put it on a `PATH` that your login shell does not read,
and the fix is in the CLI's install rather than here.

Restart NodeFloor after installing a CLI; it looks for them at startup.

## An agent's terminal is garbled

Dragging the window between monitors of different sizes can leave xterm's idea
of the grid out of step with the process. **Restart & Continue** on the node
recreates the terminal and reattaches the same session, so the conversation is
not lost.

## The update check failed

Releases live on this repository. If a check fails, the usual cause is no
network or a proxy that blocks `api.github.com`; the app never blocks on it, and
you can always download the latest build from
[Releases](https://github.com/OmorDeveloper/nodefloor-app/releases/latest) by
hand.

## A workflow is not running

Templates **arrive switched off** and list what they still need — an account, an
id, a schedule — before they will run. Check the workflow says it is on, and
that whatever it names as missing has been filled in.

## A WhatsApp document said it sent, but nothing arrived

Almost certainly the 24-hour window. The WhatsApp Business API only delivers a
free-form message inside the 24 hours after the recipient last messaged you.
Outside it, the API accepts the message, returns an id, and delivers nothing —
so the app cannot tell the difference and reports the success it was given.

Ask them to message you first, then send. Email and Telegram have no such rule.

## An email with an attachment was rejected

Attachments are base64-encoded on the wire, which is four bytes for every three,
so a 25 MB server limit is reached by about 18.7 MB of actual file. The app
refuses anything over 18 MB for email before uploading it. Telegram takes 50 MB
and WhatsApp 100 MB, so a large file can often go there instead.

## Something else

Open an [issue](https://github.com/OmorDeveloper/nodefloor-app/issues) with the
version from the title bar and what you expected to happen. If it involves a
crash, the app writes a log inside your home folder — attach it, but read it
first: it can contain paths and prompts from your own work.
