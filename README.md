# Duckboard

Duckboard is a generic and extensible ticket system that can be used in various
ways, e.g. as a kanban board. Duckboard does have a web UI, but it also has a
clean REST API that allows automation and integration with other tools.

XXX better explanation

## Concepts

Since duckboard is built in a very generic way, you really need to understand
the fundamental concepts on which it is built. Fortunately these are very
simple, but hopefully powerful at the same time.

### Domains

A single duckboard can handle multiple "domains", which are entirely separate
from each other. An action in one domain never affects anything in another
domain. This is often used to track separate work for separate projects or
teams.

Domains have a textual ID that has to be unique, but can be chosen by the
creator of the domain. This is typically a mnemonic or abbreviation of the team
or project that the domain is used for. 

Domains also have a textual description.

### Tickets

Each domain holds a number of tickets, which is what a user of duckboard will
mostly be interacting with. All tickets have a ID unique within the domain, a
title, a body text and a set of tags. Tags are the primary way to organize
tickets within duckboard, see below for more on that. Tickets also have a 
timestamp of the last modification. This is often not visible to the user, but
very important to understand how duckboard works internally. Please see below
for details.

### Tags

Tags are used to group and organize tickets within a domain. A tag can be a
key/value pair, or just a key which is just a shorthand form for key:true. The
characters that can be used in tag keys and values is restricted to make it easy
to pass tags (or filters) in URLs:

  unreserved    = ALPHA / DIGIT / "-" / "." / "_" / "~"
  tag           = +(unreserved)
                / +(unreserved) ":" +(unreserved)
  tags          = tag *( ";" tag)

Duckboard uses tag filter expressions in many places, these are built on top of
the tag definition above:

  predicate     = tag
                / "!" tag
  filter        = predicate
                / "(" predicate ")"
                / predicate "+" predicate
                / predicate "/" predicate

XXX example for tags and filters

### Boards

Boards are a way to visualise and interactively interact with tickets in a
domain. Each domain can have multiple boards, but it is important to understand
that all boards of a domain work on the tickets of the domain. Consequently
actions on one board will be reflected on different boards of the same domain.

## API

- GET /api/v1/domains
- GET /api/v1/domain/<domain-id>
- PUT /api/v1/domain/<domain-id>

- GET /api/v1/domain/<domain-id>/boards
- GET /api/v1/domain/<domain-id>/board/<board-id>
- PUT /api/v1/domain/<domain-id>/board/<board-id>

- GET /api/v1/domain/<domain-id>/tickets
- GET /api/v1/domain/<domain-id>/ticket/<ticket-id>
- PUT /api/v1/domain/<domain-id>/ticket/<ticket-id>
- POST /api/v1/domain/<domain-id>/tickets

XXX WATCH
