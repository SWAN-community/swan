# ![](https://raw.githubusercontent.com/SWAN-community/swan/main/images/swan.128.pxls.100.dpi.png)

# Secured Web Addressability Network (SWAN) - Technical

## Abstract

SWAN is a solution to generate and share pseudonymous identifiers and other data
across multiple processors in a way that is verifiable and transparent. SWAN
exists as a series of specifications and a concrete implementation in the Go
programming language.

Identifiers are shared across multiple domains within the web browsers. The
initial implementation of SWAN uses cookies (first party) written and read via
the primary navigation of the web browser. This feature is implemented in the
open-source project [Shared Web InFormaTion
(SWIFT)](https://github.com/SWAN-community/swift). It is expected other data sharing
mechanisms will be supported in time including, cross domain cookies (third
party), pop-up windows that obtain the information and return control to the
caller, browser extensions or browser features that enable cross domain data
sharing. If the web browser does not support any of the needed features the user
can be notified, and corrective action recommended such as enabling cookies or
disabling another feature of the web browser.

Supply chains supporting publishers and advertisers become auditable. Each
processor must sign the transaction to indicate they have received the data.
They may optionally add additional information alongside their signature. Any
processor that does not sign, or tamper with the transaction is immediately
identifiable to the rest of the supply chain. People can easily verify who had
access to their pseudonymous identifiers, preferences and other data enabling
them to exercise their rights under laws such as GDPR. This feature of SWAN is
supported via [Open Web ID (OWID)](https://github.com/SWAN-community/owid), a
lightweight cryptographic processor identifier. The Internet Advertising Bureau
Technology Lab (IAB TL) [Supply Chain
object](https://github.com/InteractiveAdvertisingBureau/openrtb/blob/master/supplychainobject.md)
provides the necessary support for transmitting this information.

SWAN has been designed to overlay onto existing solutions that support
programmatic advertising that currently rely on third-party cookies. It is
expected SWAN can be added to [Prebid](https://prebid.org/) and [Open
RTB](https://www.iab.com/guidelines/openrtb/) version 2.5 and above.

## Design

SWAN has been built on the open-source projects [Shared Web InFormaTion
(SWIFT)](https://github.com/SWAN-community/swift) and [Open Web Id
(OWID)](https://github.com/SWAN-community/owid).

SWIFT supports the sharing of data within the web browser across domains using
first party cookies and redirects. SWAN could utilise additional cross domain
data sharing mechanisms in the future. For example, web browser may introduce
the ability for some domains to be grouped together as a set to share
information.

OWID ensures all processors that receive the pseudonymous identifiers and other
data associated with programmatic advertising can be identified.

## Global Privacy Control (GPC)

SWAN has been designed so that the data storage components, defaults and
possibly parts of the user interface can be implemented in the web browser via
the “settings” menu. This is a model for consent preferences championed by
privacy experts in the [Global Privacy
Control](https://globalprivacycontrol.github.io/gpc-spec/) (GPC) specification.

SWAN attempts to incorporate the goals of GPC in a way that all processors
involved in the advertising supply chain can implement, and people can verify
their choices were respected.

## Implementation Considerations

### Standards

See SWIFT and OWID for information on the technical standards used to support
SWAN.

### Open RTB

As SWAN requires the use of the Supply Chain object and this is only supported
in version 2.5 and above SWAN will only be available to participants who operate
this version of Open RTB. Given the disruption already present in the Open RTB
eco system due to changes by web browser vendors this appears to be an
acceptable requirement.

### Consent Management Platforms (CMP)

CMPs utilising SWAN would store and share common information. As such CMP A
could capture information and it could be retrieved by CMP B. CMPs can store any
additional information in any way they wish. CMPs benefit from a common and
shared data model.

## Out of Scope

### Compliance

Complying with GDPR or any other law is a matter for web authors, data
controllers or data processors privacy counsel and is solely a matter for
implementors and is not within the scope of OWID.

### Encryption

The data held within the OWID payload does not need to be encrypted. It is left
to the creator of the OWID to determine the payload data, if any, and whether it
is encrypted.

### Key management

Implementors must ensure private keys are stored securely with limited access
but are free to determine how to achieve this.

Note: The implementation contains features to reduce the need for operational
personnel to be exposed to sensitive keys. Knowledge of Open SSL or command line
tools is not a requirement to operate.
