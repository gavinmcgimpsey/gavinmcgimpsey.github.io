---
layout: post
title: "Rails resources Cheat Sheet"
---

There are seven common actions that Rails expects to be executed on a type of web resource:

+ `index`: list the resources of the type 
+ make a new resource, that is:
  + `new`: seek input from the user about the new resource 
  + `create`: add the new resource using that input 
+ work with a specific resource, in one of a few ways:
  + `show`: show it 
  + `destroy`: delete it 
  + change it, that is:
    + `edit`: seek input from the user about the change 
    + `update`: update the resource using that input 

These seven actions correspond to different routes that Rails' `resources` controller helper function will set up. In learning these, I found it helpful to look at the relationships involved from a few different angles. Personally, things made the most sense when organized around the different HTTP verbs being used.

## HTTP Verbs

| HTTP Verb | Action | Path |
|---|---|---|
| GET | `index` | /photos |
| | `show` | /photos/:id |
| | `new` | /photos/new |
| | `edit` | /photos/:id/edit |
| POST | `create` | /photos |
| PATCH/PUT | `update` | /photos/:id |
| DELETE | `destroy` | /photos/:id |

## Routes

| Path | HTTP Verb | Action |
|---|---|---|
| /photos | GET | `index` |
| | POST | `create` |
| /photos/new | GET | `new` |
| /photos/:id | GET | `show` |
| | PATCH/PUT | `update` |
| | DELETE | `destroy` |
| /photos/:id/edit | GET | `edit` |

## Actions

| Action | Path | HTTP Verb |
|---|---|---|
| `index` | /photos | GET |
| `new` | /photos/new | GET |
| `create` | /photos | POST |
| `show` | /photos/:id | GET |
| `destroy` | /photos/:id | DELETE |
| `edit` | /photos/:id/edit | GET |
| `update` | /photos/:id | PATCH/PUT |

## CRUD

Another angle on this is to consider the four CRUD actions - Create, Read, Update, and Destroy:

| Action | Path | HTTP Verb |
|---|---|---|
| Create |||
| `new` | /photos/new | GET |
| `create` | /photos | POST |
| Read |||
| `index` | /photos | GET |
| `show` | /photos/:id | GET |
| Update |||
| `edit` | /photos/:id/edit | GET |
| `update` | /photos/:id | PATCH/PUT |
| Destroy |||
| `destroy` | /photos/:id | DELETE |

