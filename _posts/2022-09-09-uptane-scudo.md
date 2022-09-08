---

layout: article
title: "Scudo: End-to-End Vehicle Software Security from Uptane and in-toto"
subnav: blog
comments: true
tagline: "This spring, the [Uptane](https://uptane.github.io/) project introduced Scudo, a comprehensive secure framework that can deliver end-to-end software supply chain protection for computing units on automobiles."
author: '<a href="/people#lois_delong">Lois Anne DeLong</a>'
categories:
  - '<a href="/projects#uptane">Uptane</a>'

---

This spring, the [Uptane](https://uptane.github.io/) project introduced Scudo, a comprehensive secure framework that can deliver end-to-end software supply chain protection for computing units on automobiles. Named after the Italian word for shield, Scudo integrates the compromise resilience and secure delivery mechanisms of Uptane with the proven supply chain security mechanism of in-toto. The resulting framework offers a timely response to threats against an emerging attack surface—automotive electronic control units or ECUs— at a point in time when both industry standards and government regulations are calling for improved protection of the software lifecycle across all industries.  

As described in [Scudo: A Proposal for Resolving Software Supply Chain Insecurities in Vehicles](https://uptane.github.io/papers/scudo-whitepaper.pdf), an Uptane whitepaper originally published May 22, 2022, and updated in July, the framework ensures that the images being uploaded by the Uptane framework are free of tampering. It can offer this assurance because of the signed metadata in-toto generates at each step in the development, packaging, testing and delivery of a software image. This metadata attests to the authenticity of the image and allows a client to verify who performed each step and in what order. If the signature or the information in the metadata is different from what was intended, Scudo will reject it. 

Scudo brings to the solution of supply chain insecurity two established open source technologies. For the past five years, Uptane has been a mainstay in secure software update systems used by a number of original equipment manufacturers (OEMs). While in-toto is new to the automotive space, it has been widely integrated into open source projects, such as [Sigstore](https://docs.sigstore.dev/cosign/attestation/), [GitLab](https://github.com/in-toto/friends/tree/main/gitlab), and [Reproducible Builds](https://github.com/in-toto/friends/tree/main/rebuilderd). Even SolarWinds, which ignited much of the recent concern about supply chain vulnerabilities when its software update mechanism inadvertently introduced malware that led to massive data breaches, [adopted in-toto as part of its re-designed security system](https://www.solarwinds.com/resources/whitepaper/setting-the-new-standard-in-secure-software-development-the-solarwinds-next-generation-build-system/delivery). in-toto is also a core part of [SLSA](https://github.com/in-toto/friends/tree/main/slsa), the industry’s leading software supply chain best practices framework.

## How Scudo works

The whitepaper offers a very high-level conceptual design of how Scudo works (see diagram). A supply chain orchestrator signs the image to be uploaded, and its associated in-toto metadata, and maps the relevant metadata to a corresponding layout. As the name implies, the layout defines the steps of a software supply chain that must be carried out in order to write, test, package and distribute your software. Put simply, the metadata says what was done while the layout states what was supposed to be done. Agreement between the two indicates the client is receiving a secure copy of the requested software. 

<img align="center" src="/img/blog/scudo_figure_1.jpg" style="margin: 0px 20px"/>

**Figure 1:** *Scudo modifies the standard Uptane structure by introducing in-toto metadata into one of the repositories. In this example, we have assumed in-toto metadata is stored alongside the images in the Image repository.*

Scudo is built on a solution successfully implemented by [Datadog](https://www.datadoghq.com/) that uses both in-toto and [The Update Framework (TUF)](https://theupdateframework.io/), Uptane’s parent framework. The Datadog solution is used to secure hundreds of integrations for its Agent—a product that collects metrics for analysis of host machines. (You can read about this in-toto/TUF collaboration in a [Datadog blog](https://www.datadoghq.com/blog/engineering/secure-publication-of-datadog-agent-integrations-with-tuf-and-in-toto/), which was written by Scudo team member [Trishank Karthik Kuppusamy](https://www.linkedin.com/in/trishank-karthik-kuppusamy/). The specification was also submitted and approved as an [in-toto enhancement (ITE)](https://github.com/in-toto/ITE/blob/master/ITE/2/README.adoc),
and serves as guidance for implementing compromise-resilient continuous integration/continuous pipelines). 

According to Kuppusamy, “Integrating in-toto with TUF allowed us to build a compromise-resilient CI pipeline three years before the SolarWinds incident. The combination protected our Agent integrations against man-in-the-middle attacks anywhere in the software publication lifecycle, from the moment our developers release new or updated integrations to when our end-users install them. The combination of in-toto and TUF uses defense in depth to an extent where these attacks can be rendered infeasible.”

As shown in the diagram below, Scudo stores in-toto metadata in the Image repository, one of two repositories employed in Uptane’s architecture. The dual repository design is a major difference between Uptane and TUF, and therefore determining which should be used to store in-toto metadata was one of the first design decisions the Scudo team addressed. While either repository can actually be adapted to this purpose, the Image repository, which is signed by offline keys, offers the more secure option. 

## Creating Defense-in-Depth with other open source strategies

In presenting Scudo, the whitepaper authors—Kuppusamy; [Aditya Sirish A Yelgundhalli](https://engineering.nyu.edu/student/aditya-sirish-yelgundhalli), [Marina Moore](https://cyber.nyu.edu/profile/marina-moore/), [Lois Anne DeLong](https://www.linkedin.com/in/lois-delong-0706a128/), and [Justin Cappos](https://ssl.engineering.nyu.edu/personalpages/jcappos/) of New York University; and [Santiago Torres-Arias](https://www.cerias.purdue.edu/site/people/faculty/view/3153) of Purdue University—acknowledge a number of other open source strategies that could perhaps be adapted to an automotive space. The paper presents an overview of these technologies, which include [Sigstore](https://www.sigstore.dev/), [Grafeas](https://grafeas.io/), and [SBoM](https://www.ntia.doc.gov/files/ntia/publications/sbom_minimum_elements_report.pdf) formats, such as [SPDX](https://spdx.dev/) and [Cyclone DX](https://github.com/CycloneDX), and shows how Scudo, through in-toto, offers some distinct advantages over these choices. For one thing, in-toto has the ability to cryptographically track artifacts through the full supply chain. It also includes primitives that serve to define and enforce policies.

Yet, the authors also point out that, like both Uptane and in-toto, Scudo can work as a complementary element in other systems. "Since its inception, in-toto was intended to close the gap between software repositories and the developer,” explains Torres-Arias, who was the lead developer of in-toto while completing his doctorate at New York University. “However, to provide some symmetry, other solutions like TUF and Uptane work fantastically at closing the gap between software repositories and software users. Because of this, these solutions are great when used together." A multi-layered framework in which Scudo is paired with other solutions, can help to create “Defense-in-Depth” which as defined by [US NIST IR8183](https://nvlpubs.nist.gov/nistpubs/ir/2017/NIST.IR.8183.pdf) is “the application of multiple countermeasures in a layered or stepwise manner” to “ensure that attacks missed by one technology are caught by another.”

## Next steps for Scudo
The Scudo team is planning to publish a more formal specification of the framework as a [Proposed Uptane Revisions and Enhancements (PURE)](https://github.com/uptane/pures). The PURE document will propose changes to the [Uptane Standard](https://nvlpubs.nist.gov/nistpubs/ir/2017/NIST.IR.8183.pdf)  that could expand Scudo’s use as a defense against the software supply chain threats in real-world applications. This expanded version of the Scudo specification will more closely examine the unique demands of the automotive industry, such as dealing with large and diverse codebases, and the reality that ECUs vary widely in terms of bandwidth and other resources.

According to Prof. Justin Cappos “It is essential that a viable software supply chain solution exists before an incident like SolarWinds impacts the automotive community, where it could cost many lives.”