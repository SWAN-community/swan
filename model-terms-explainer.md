![Secure Web Addressability Network](images/swan.128.pxls.100.dpi.png)

# Secure Web Addressability Network (SWAN) - Model Terms Explainer

**Version: 1.0**

**1.  Introduction**

SWAN is designed to improve people’s important privacy rights while
simultaneously improving their access to a wide diversity of ad-funded
publishers.

It is an established legal principle that people have inalienable rights over
their personal data that must be respected by any organisation seeking to
collect and process it. For this reason, it is enshrined in data protection laws
that people sharing their personal data do so on the basis that they have
knowledge, awareness and, in the present context, choice about why and how their
personal data is used. Among the important privacy rights are consent to the use
of personal data for the type of personalized marketing in question, the right
to determine when that use ends, and the right to be forgotten.

The use of personal data to provide ad-funded access to online services is a
well-established legitimate interest across the creative commons. Enhancing both
digital experiences and the relevance of the content people interact with
provides additional unquestionable benefits to people from this use.

Because it is unreasonable for people to understand the entire supply chains of
every organization they interact with, we require a method of providing people
appropriate notice and control, while still enabling their friction-free access
to digital services. As a corollary, if we impair publishers’ access to the
supply chain vendors that support their business, this would have a negative
impact on people’s access to the diversity of digital publishers available today
as well as the quality of content each of these publishers could provide. Both
of these impacts would have severe and damaging consequences for freedom of
expression, freedom of the press, and many other social benefits that rely on
open markets and competition.

SWAN provides a solution that respects the fundamental tenets of consumer choice
and data control that underpins those many frameworks without jeopardizing the
ability for small publishers to rely on supply chains to operate their business.

**2.  Aims of SWAN**

The principal aim of SWAN is to respect people’s privacy rights, whilst also
supporting choice and competition among all actors within the digital ecosystem.

SWAN is designed to preserve four fundamental rights of users in this context,
namely: (1) consent as mandatory lawful basis for such processing; (2) choice in
relation to the use of personal data; (3) transparency regarding the data supply
chain; and (4) the right to be forgotten.

SWAN provides a method to ensure every level of the SWAN Ecosystem respects
people’s fundamental privacy rights. Additionally, SWAN simplifies the complex,
lengthy and technical descriptions detailing how data is shared, who it is
shared with and on what basis by relying on standardized “Model Terms” the
benefits of which can then be reflected into a simple user-facing privacy notice
that people can understand. Given an open marketplace, by definition, does not
predetermine who will enter into any transaction, SWAN provides people an audit
log to provide transparency as to which organizations were involved with
delivery of the content or services they interact with.

People have no greater exercise of ultimate control over the continued use of
their personal data than exercising their right to be forgotten. SWAN also makes
exercising this right easy, by providing people a default temporary pseudonymous
identifier while also not interfering with people’s ability to have a
site-specific identifiers.

**3.  How the SWAN Ecosystem Works**

[Technical FAQ](https://swan.community/faqs/)

**4.  Actors within the SWAN Ecosystem**

SWAN consists of two interconnected networks: the first is the SWAN Network,
comprising Operators responsible for maintaining the mechanism that underpins
the second, wider network, that being the SWAN Ecosystem comprising Senders and
Receivers of SWAN Data.

All parties in these networks must agree to be bound by the Model Terms, which
are built on a short set of “Binding Principles”. These principles follow in the
spirit of other, light-touch frameworks established in the industry (e.g.
Partnership for Responsible Addressable Media (PRAM)) and more broadly, open
source licences, which have provided an effective self-regulatory regime across
information commons. The Binding Principles seek to achieve a balance by, on the
one hand, not being so prescriptive that the terms are unworkably inflexible
whilst, on the other, avoiding ambiguity that could cause the terms to lose any
practical meaning or undermine enforcement. In this way, the Model Terms ensure
clarity and certainty amongst those parties over their obligations without being
unnecessarily burdensome.

Parties may only make use of SWAN Data (or data linked to it) for purposes
specified in the Binding Principles. These focus on simple, intelligible use
cases universal to the advertising ecosystem. SWAN builds

Each party in these networks is responsible for whistleblowing if it becomes
aware of any bad behaviour in those networks. Breaching parties must publish
details of their wrongdoing where this cannot be fixed. To avoid breaching their
upstream agreements (which also incorporate the Model Clauses), innocent parties
must also publish those details if the breaching parties do not.

The Model Terms exclude a party’s liability to the other if it publishes a
notification in error where this was made in good faith. This is intended to
mitigate disincentives to publish details of breaches based on concerns of
reprisal by the breaching party, particularly where the latter may have greater
financial strength when compared to the notifying party.

&nbsp;&nbsp;&nbsp;&nbsp;a.  <u>SWAN Operators</u>

SWAN Operators are a closed group of operators that, for the purposes of data
protection laws, act as joint controllers of the SWAN Data they process. When
people participate in the SWAN Ecosystem, SWAN Operators write data to their
web-enabled devices to ensure connectivity with the SWAN Network. SWAN Operators
facilitate the use of SWAN Data by publishers and their supply chain, but do not
participate in the use of pseudonymous identifiers to fund access to digital
content or optimize people’s digital experiences.

&nbsp;&nbsp;&nbsp;&nbsp;b.  <u>Senders and Receivers</u>

The SWAN Ecosystem consists of many bilateral contractual relationships between
“Senders” and “Receivers”.

Senders and Receivers rely on the pseudonymous identifiers and the user’s
preferences (all elements of the “SWAN Data”) with other personal data to
communicate with one another via “Transactions”.

To maintain the pseudonymous nature of SWAN identifiers, neither Senders nor
Receivers may link the SWID with the real-world identity of the individual.

To ensure transparency, every Transaction must be cryptographically signed by
the Sender and the Receiver. Each signature in a chain of Transactions is then
added to an audit trail, which is accessible by the user through a user
interface.

Where a Sender is a “Root Party” (i.e. the party, typically a publisher,
initiating the first Transaction in a chain), it will be required to provide a
mechanism to allow Users to inspect information relating to the Transactions
involved in a particular chain (including the identities and privacy notices of
the relevant parties), known as the “SWAN Audit Log”. The Root Party is free to
decide how to fulfil its obligations to provide the SWAN Audit Log for
inspection by the User. An option for doing so might involve engaging the
services of a User Interface Provider or other service provider that also
provides an inspection interface for the SWAN Audit Log. Other options include
creating their own mechanism, using future open source providers, or where the
web browser supports such a feature at a future time, triggering that feature of
the web browser.

&nbsp;&nbsp;&nbsp;&nbsp;c.  <u>User Interface Providers</u>

“User Interface Providers”, such as website owners, consent management platforms
or browsers, must contract with at least one SWAN Operator to participate in the
SWAN Ecosystem.

User Interface Providers enable people to easily access and update any SWAN
Data. SWAN Data consists of a temporary, unique Secure Web ID or “SWID”, which
people can associate their preference to receive personalized marketing on the
current device, as well as to optionally apply across other devices when
authenticating with the SWAN Network. User Interface Providers are also bound by
the SWAN Model Terms.

User Interface Providers are the only parties who can see raw (non-hashed)
personal data, as they act as the conduit between the user and the SWAN Network
for the purpose of updating and maintaining a user’s pseudonymous identifiers
and preferences.

The User Interface Providers also provide the “User Interface” for people to
understand how and why the data they provide to SWAN is used and, on that basis,
provide their preference as to whether they wish to receive personalized
marketing. SWAN ensures consistency amongst User Interface Providers by
prescribing the form and content of that notice, but also acknowledges that User
Interface Providers may be managing other consent preferences (such as non-SWAN
related marketing preferences, or DNT preferences *etc.*). The User Interface
Provider may use the same interface for managing such other consent preferences,
but it can never combine any notice or preference question relating to SWAN with
such other consent management activities. User Interface Providers can include
third party consent management platform providers, website operators that have
developed their own such functionality or website browsers. Where the latter is
acting purely as a software vendor, separate terms will be required to align
those vendors to the SWAN model as the Model Terms only bind Senders and
Receivers of SWAN Data.

The Model Terms acknowledge that User Interface Providers may also wish to use
SWAN in a different capacity. To do so they will need to enter into a separate
contract incorporating the Model Terms which will govern that relationship with
parties within the SWAN Ecosystem.

**5.  Users**

SWAN adopts a privacy-centric design that puts the needs of Website visitors
(“Users”) at the heart of the Model Terms.

To give effect to Users’ right to be forgotten the SWID can be reset by users.
The SWID also resets at regular periods defined by the SWAN Operators, providing
further automatic protection.

To provide Users will choice and control, affirmative ‘opt-in’ consent is
required for personalized marketing generally. Users can withdraw that consent
and can also block specific content if desired. The preference option also
resets at regular periods, initially prescribed at 2 years based on regulatory
guidance but ultimately defined by the SWAN Operators. As the only access to
their devices in relation to SWAN is strictly necessary for these controls to be
achieved, we do not consider that the legal requirements concerning consent for
cookies and similar technologies apply. User Interface Providers are obligated
under the Model Terms to accompany preference functionality with important
privacy compliance information and to take steps to ensure clarity where a User
Interface provides functionality unrelated to SWAN.

To ensure Users’ right to be informed is respected, the Model Terms require
privacy notices of each of the Parties involved in delivering content to be made
accessible to Users. As SWAN Operators are controllers themselves (working
jointly) with their own legal notice requirements, User Interface Providers must
provide a link to those operators’ joint privacy notice. It is intended that
this notice and the SWAN Operator Agreement will be produced and published prior
to completion.

Users can also opt-out of receiving specific content and this preference
information will be linked to its SWID.

**6.  Duty to report SWAN violations**

The SWAN Ecosystem is built on many bilateral contractual relationships between
Senders and Receivers based on the Model Terms. Thus, a breach of one bilateral
contract may result in a breach of the preceding bilateral contract and so on,
until that breach is rectified. This is important as it creates an incentive for
Senders (who are Receivers under the contract for the immediately preceding
Transaction) to ‘name and shame’ violations in their transactional chain so as
to avoid themselves breaching the Model Terms further up the chain with the
preceding Sender.

All parties to the Model Terms share a collective responsibility to uphold the
principles of SWAN and to call out any detected violations of the Model Terms.
Any party becoming aware of a breach should notify the appropriate government
authorities of the illicit activity, where the SWAN audit log may be used as
supporting evidence.

**7.  Participating in the SWAN Ecosystem**

Any organisation can participate in the SWAN Ecosystem on equal terms, so long
as they abide by these SWAN Model Terms. All SWAN Operators must offer
conditions of access that are just, reasonable, and non-discriminatory.

The Model Terms can be executed as a standalone agreement or incorporated into a
broader agreement containing commercial and other terms. The Model Terms are
governed by the governing law stated in that broader agreement, or where they
stand alone, the law of the jurisdiction in which parties are located (or
England where they are based on different locations).

**8.  Alignment of SWAN Model Terms with Data Protection Laws**

SWAN adopts privacy by design. Its key features respect fundamental and
universal data protection principles. Consent-based preferences, ‘one-touch’
right to be forgotten and ‘model’ contractual terms that apply between each
party in the SWAN ecosystem are all borne from this ‘privacy-first’ approach.
The SWAN Model Terms also respect those principles. For example, they
incorporate by default mandatory processor and transfer provisions to support
compliance in the relevant legal jurisdictions where these are required.

In taking the approach above, it is acknowledged that SWAN takes a novel
interpretation to certain areas of the EU/UK GDPR and other data protection
laws, which may require conventional approaches and guidance to be revisited.
These areas will require discussion and agreement with regulators, legislators
and the wider privacy community.

**9.  Changes to the Model Terms**

From time to time it may be necessary to update and/or amend the Model Terms,
such as to account for changes in data protection laws, or to reflect new
features and functionalities introduced to SWAN.

All changes to the Terms are subject to a change control process and require
approval from SWAN’s overarching governance and oversight groups. Except in rare
circumstances, all participants in SWAN will receive advance notice of any
updates to the Model Terms to allow time for adjustment. Changes to the Model
Terms that relate to core aspects of the SWAN data or model will require
explicit acceptance before Parties are bound to them (failing which the
agreement will terminate). In all other cases, changes notified will
automatically apply after the notice period expires.

No changes to the Model Terms will have retrospective effect and changes will
only apply to the sharing and use of SWAN Data after the amended Terms take
effect.
