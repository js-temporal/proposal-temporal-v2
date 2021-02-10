# Future Additions to Temporal

## Status

Champion(s): Currently seeking a champion

Author(s): N/A

Stage: 0

## Motivation

This proposal is a place to collect feedback from developers using Temporal, and to develop ideas which didn't make it into the original Temporal proposal.

While working on the [Temporal](https://github.com/tc39/proposal-temporal) proposal, the proposal champions gathered user feedback via a survey from over 50 JavaScript developers who had tried out the proposed API, as well as via interviews, and informally from people who had commented or added emoji reactions to GitHub issues.

There were many suggestions and features that were not included in the original Temporal proposal for one or more reasons:
- Seemed useful, but not essential to the success of an already large proposal.
- Could be implemented in userland in the meantime.
- Could be implemented in the future in a web-compatible way.
- Would be useful to see how (and whether) the community fills the feature gap before designing a built-in equivalent.

As Temporal progresses towards implementation in browsers and elsewhere, and becomes more and more widely used, it's likely that more developers will have feedback about its rough edges and sticking points.
Collecting feedback is a good way to decide which of those are worth addressing.

## Use cases

To get started, here are some short summaries of features that had been discussed at some point during the development of Temporal.
These features aren't definitive, and this proposal may eventually encompass more or fewer or different ones.
During the discussion phase of this proposal, each feature should be described clearly, and should include comparisons to other popular date/time libraries.

Here are some of the features that were most often requested but not included in Temporal:

### Ability to parse a user-supplied string format into Temporal objects

This was the most frequently requested feature that Temporal doesn't include, and is a popular feature in third-party libraries.
Moment (`moment('07/28/2020', 'MM/DD/YYYY')`), Luxon (`DateTime.fromFormat('07/28/2020', 'MM/DD/YYYY')`), and date-fns (`parse('07/28/2020', 'MM/dd/y', new Date())`) can all do this.

An example would be parsing an [RFC 5322](https://tools.ietf.org/html/rfc5322) string (`Fri, 21 Nov 1997 09:55:06 -0600`), or conversion of existing string data from another system into a form that Temporal can work with.

Currently Temporal's answer is that if you need to parse anything other than ISO format, you should write your own parser.
This is easy to get wrong, and is out of reach for some beginning developers.
Additionally, this parsing should be locale-aware if it includes month names, which would mean that a third-party library would be a large payload to include all the locale data if used in a web app.

### An interval type, or some other way of iterating through a range

Another feature that has been requested more than once is the ability to iterate with a certain step through a range between two instances of the same Temporal type.
For example, a loop visiting every day of the interval between two `Temporal.PlainDateTime` instances.

### Ability to format a Temporal object into a user-supplied string format

Similar to parsing, this is a popular feature in third-party libraries, though formatting is much easier to build correctly in user code than parsing is, so it was requested less urgently.

### More idiomatic comparison methods

The `compare()` static methods have confusing parameter order, in order to be able to be used as the comparator function for `Array.prototype.sort()`.
Temporal types could have more idiomatic methods named something like `lessThan()`, `greater()`, `isBefore()`, `isSameOrAfter()`.
Although these would duplicate the functionality of `compare()`, they would make user code much easier to read.

## Proposal

You can browse the [rendered proposal](https://js-temporal.github.io/proposal-temporal-v2/) (currently only an introduction) or browse the [source](https://github.com/js-temporal/proposal-temporal-v2/blob/main/spec.emu).

## Polyfill

This repository should include polyfills for each feature under consideration.

## Q&A

**Q**: Why do we have this, can't these things just be included in the Temporal proposal?

**A**: The Temporal proposal needed to have a stable feature set in order to advance, so the champions had to draw the line somewhere.
Many of the ideas sounded good but weren't worth delaying the Temporal proposal over, or making a large proposal larger (and therefore more difficult to review or implement) than it already was, but would be great as a later addition.

**Q**: Can't all these things just be done in user code or in libraries that user code includes?

**A**: Some of them can, but if there are operations related to dates and times that are commonly done and non-trivial to do correctly, then they should be considered for inclusion in ECMAScript.
It would be bad if the world used Temporal but also more often than not had to include a "Temporal helpers" library.
