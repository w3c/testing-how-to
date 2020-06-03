# Web-Spec Testing How-To

This how-to provides information useful to anyone doing testing for specifications for the Web, with some focus on
giving guidance to W3C working groups on how to do spec testing toward the goal of transitioning their specifications
out of Candidate Recommendation (CR). It answers these questions:

* [What’s the goal of web-spec testing]?

* [How does testing relate to W3C “CR exit criteria”]?

* [How to decide what tests are needed for a spec]?

* [How to produce the tests needed for a spec]?

## What’s the goal of web-spec testing?

The goal of web-spec testing is to help achieve **interoperability** among different implementations of a web spec.

The means for providing evidence to evaluate interoperability for a spec, and the means to *document*
interoperability for a spec—or lack of interoperability—is to write test cases for all requirements in the spec.

So *web-spec testing* means **writing tests for requirements in web specs, and running those tests**.

### web-platform-tests

[web-platform-tests](https://github.com/w3c/web-platform-tests) is a repository of tests for dozens of specs—the
majority being for features of the [web runtime].

[web-platform-tests](https://github.com/w3c/web-platform-tests) also provides test infrastructure that includes test
harnesses and a self-hosted web server which enables you to write custom backend code (in Python) to control server
behavior for tests—for example, for tests that expect particular HTTP headers, or that otherwise expect certain types
of server responses, and so on.

W3C working groups maintain their tests in the [web-platform-tests] (https://github.com/w3c/web-platform-tests) repo,
using the infrastructure provided.

## How does testing relate to W3C “CR exit criteria”?

In order to publish a Candidate Recommendation of a spec at the W3C, working groups are required to [“document how
adequate implementation experience will be demonstrated”] for the spec.

[“document how adequate implementation experience will be demonstrated”]:
https://www.w3.org/2017/Process-20170301/#candidate-rec

W3C working groups deal with that requirement by including in the Status section of the spec a statement about the
specific **CR exit criteria** the spec must meet to be able to exit from CR—and the spec exits from CR when the
working group demonstrates that the spec has met the CR exit criteria the group has set for it.

W3C working groups can choose to set whatever CR exit criteria they want for a spec—as long at it demonstrates
adequate implementation experience—but in practice, working groups commonly choose to define the exit criteria for a
spec as being that there are least **two interoperable implementations of each feature in the spec**.

So a common way for a W3C group to handle the requirement to [“document how adequate implementation experience will be
demonstrated”] is to include a statement like the following in the Status section of a spec:

> This Candidate Recommendation will have met its exit criteria when there are at least two interoperable
> implementations of each feature in the spec, documented in an implementation report.

No implementation report needs to exist at the time a CR version of a spec is published. Groups can use the CR phase
to develop the implementation report.

And the way that groups develop implementation reports to document they’ve met their CR exit criteria is by first
**writing tests and running those tests against multiple implementations**, then putting the results into the report.

## How to decide what tests are needed for a spec?

The process of testing a spec begins by ensuring the spec contains testable requirements. For advice on writing those
kinds of requirements into a spec, a number of how-to guides are available:

* [QA Framework: Specification Guidelines](https://www.w3.org/TR/qaframe-spec/)
* [A Method for Writing Testable Conformance Requirements](https://www.w3.org/TR/test-methodology/)
* [Writing specifications: Kinds of statements](https://ln.hixie.ch/?start=1140242962&count=1)

Testing the requirements in a spec means testing the available implementations for the spec—which is a means to
document the [implementation experience] for the spec, based on the **features** in the spec.

> Implementation experience is required to show that a specification is sufficiently clear, complete, and relevant to
> market needs, to ensure that independent interoperable implementations of each feature of the specification will be
> realized

[implementation experience]:
https://www.w3.org/2017/Process-20170301/#implementation-experience

So the task of evaluating what tests are needed for a particular spec begins by determining what features a spec
supplies. Features for the [web runtime] are defined using these mechanisms:

* [RFC 2119 keywords]
* [WebIDL blocks]
* [ABNF]
* [HTTP header definitions]
* [Algorithms]
* [Elements, attributes, and attribute-value definitions]
* [CSS property definitions]

Other web features can be defined using these mechanisms:

* [Vocabulary definitions]

The following sections provide specific guidelines on how to look at a spec to determine which of the above mechanisms
it uses, and so what features from the spec need to be tested.

### RFC 2119 keywords

[RFC 2119](https://tools.ietf.org/html/rfc2119) defines keywords such as “must” and “must not” to be used in specs to
define requirements for implementations. All W3C specs, regardless of the particular constructs they employ to define
features, have at least some requirements specified using RFC 2119 keywords.

So wherever a spec uses RFC 2119 keywords, the spec is defining mandatory behavior of a feature that implementations
are required to follow; and thus, evaluating implementations of the spec necessitates writing tests for each
statement that uses an RFC 2119 keyword.

If the implementations for which a spec defines features are browser engines and the RFC 2119 keywords in the spec
define requirements observable from JavaScript code running in a browser, then the [web-platform-tests] infrastructure
has an extensive set of methods that can be used to write tests for the requirements.

### WebIDL blocks

W3C specs that define programming interfaces employ [WebIDL](https://heycam.github.io/webidl/) as a formalism: Specs
contain WebIDL “blocks” that use the WebIDL formalism to specify the “API shape” for an interface, including how
instances of the interface are instantiated/constructed, and the methods and properties that prototypes and instances
of the interface provide.

So the features such specs provide include capabilities and information exposed by methods and properties the
interfaces supply—and thus if is a spec contains WebIDL blocks, the tests necessary to evaluate implementations need
to include tests of conformance to the requirements formally defined in all the WebIDL blocks in the spec.

The [web-platform-tests] infrastructure provides a specific test harness, [idlharness.js], for writing WebIDL tests.

[idlharness.js]:
http://testthewebforward.org/docs/testharness-idlharness.html

### ABNF

[to come]

### HTTP header definitions

[to come]

### Algorithms

Specs don’t just use formalisms such as [WebIDL blocks] or [ABNF] to specify functional behavior or syntax, nor do
specs always define all other requirements using statements with explicit [RFC 2119 keywords]. Many requirements are
also often found in steps within multi-step algorithms given for implementations to follow.

While some steps in some algorithms might have [RFC 2119 keywords], many do not. Instead the algorithms might be
preceded by a statement saying something like, *“Conforming implementations must follow this algorithm:”*—or the
algorithms might even be preceded with a statement just saying something like, *“Run these steps:”*.

So each algorithm in a spec must be closely examined to identify the exact behavior of the resulting feature the
algorithm is part of—and thus, evaluating implementations of the spec necessitates writing tests which cause each
algorithm in the spec to be exercised, and that then evaluate any observable results of each step of each algorithm.

If the implementations for which a spec defines features are browser engines and the algorithms in the spec define
results that are observable from JavaScript code running in a browser, then the [web-platform-tests] infrastructure
has an extensive set of methods that can be used to write tests for the features covered by the algorithms.

### Element, attribute, and attribute-value definitions

[to come]

### CSS property definitions
[to be contributed by somebody who’s familiar with writing CSS tests/specs; of course should mention reftests]

### Vocabulary definitions
[to be contributed by somebody who’s familiar with writing specs and tests for vocabularies]

## How should my group go about producing tests for meeting CR exit criteria?

This section provides details about putting logistics into place for producing tests. 

### Test facilitator

For each spec, assign a person to be the “test facilitator” or “test czar” for that spec, and give that person the authority to make decisions, and the responsibility for ensuring the necessary tests are developed.

### Test as you commit

All normative spec changes are generally expected to have a corresponding pull request in [web-platform-tests](https://github.com/w3c/web-platform-tests), either in the form of new tests or modifications to existing tests, or must include the rationale for why test updates are not required for the proposed update.

Typically, both pull requests (spec updates and tests) will be merged at the same time. If a pull request for the specification is approved but the other needs more work, add the '[needs tests](https://w3c.github.io/spec-labels.html)' label or, in web-platform-tests, the '[status:needs-spec-decision](https://github.com/w3c/web-platform-tests/issues?utf8=%E2%9C%93&q=label%3Astatus%3Aneeds-spec-decision%20)' label. Note that a test change that contradicts the specification should not be merged before the corresponding specification change.

If testing is not practical due to web-platform-tests limitations, please explain why and if appropriate file an issue with the '[type:untestable](https://github.com/w3c/web-platform-tests/issues?utf8=%E2%9C%93&q=label%3Atype%3Auntestable%20)' label to follow up later.

[What’s the goal of web-spec testing]:
#what-is-the-goal-of-web-spec-testing

[How does testing relate to W3C “CR exit criteria”]:
#how-does-testing-relate-to-w3c-cr-exit-criteria

[How to decide what tests are needed for a spec]:
#how-to-decide-what-tests-are-needed-for-a-spec

[How to produce the tests needed for a spec]:
#how-to-produce-the-tests-needed-for-a-spec

[web-platform-tests]:
#web-platform-tests

[RFC 2119 keywords]:
#rfc-2119-keywords

[WebIDL blocks]:
#webidl-blocks

[ABNF]:
#abnf

[Algorithms]:
#algorithms

[HTTP header definitions]:
#http-header-definitions

[Elements, attributes, and attribute-value definitions]:
#elements-attributes-and-attribute-value-definitions

[CSS property definitions]:
#css-property-definitions

[Vocabulary definitions]:
#vocabulary-definitions

## Other resources

* [QA Framework: Specification Guidelines](https://www.w3.org/TR/qaframe-spec/)
* [A Method for Writing Testable Conformance Requirements](https://www.w3.org/TR/test-methodology/)
* [Writing specifications: Kinds of statements](https://ln.hixie.ch/?start=1140242962&count=1)

<!-- -*- fill-column: 118 -*- vim: set textwidth=118 : -->
