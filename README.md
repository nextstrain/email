# Emails from Nextstrain

Software for sending out emails to Nextstrain users.

Initially motivated by need to send out emails to early adopters of Nextstrain
Groups.


## Setup

Install and setup required software in _.env/_.

    ./devel/setup

You only need to do this once, or whenever you want to refresh your development
environment.  Either [Micromamba][], [Mamba][], or [Conda][] must be available
for setup to succeed.

[Micromamba]: https://mamba.readthedocs.io/page/user_guide/micromamba.html
[Mamba]: https://mamba.readthedocs.io
[Conda]: https://docs.conda.io/projects/conda/


## Authoring

Emails are written as _message kits_.  Each kit is a directory containing:

  - a _manifest.yaml_ file describing the message
  - associated template files for HTML and/or plain text parts, attachments, etc.

Kits are stored in _drafts/_ while we're writing them.  After they're sent out
(see below), they should be moved into _sent/_ so it's clear what's been sent
out and what's not.

### Manifests

See existing kits to learn by example, or refer to
<https://metacpan.org/pod/Email::MIME::Kit#DESCRIPTION> for now.

### Templates

Each part of a message is a template rendered via [Template Toolkit
2](https://template-toolkit.org) (TT2 or just TT).  We use only a very limited
subset of TT that you may be able to infer usage of from context here.  For
more formal usage information, see TT's documentation on
[syntax](http://template-toolkit.org/docs/manual/Syntax.html),
[directives](http://template-toolkit.org/docs/manual/Directives.html), and
[filters](http://template-toolkit.org/docs/manual/Filters.html).


## Sending

The sending process uses a _work/_ directory to allow for a two-phase process.

First, generate messages from a kit into the _work/outbox/_ maildir:

    ./bin/generate --kit drafts/some-message/ --copies recipients.ndjson

This will produce one file per message in _work/outbox/new/_.  You can review
these with a text editor or a mail program of your choice.  (I like `mutt -Rf
work/outbox/`.)

If you're happy with the messages, send them out into the world with:

    ./bin/send

This will connect to an SMTP server and record successfully sent messages in
_work/sent/_ and failed messages in _work/failed/_.

See `./bin/generate --help` and `./bin/send --help` for more details.
