# Another World

Many of the articles that I read about challenges in automated testing
don't connect with me. They often talk about the challenges of finding page
elements, detection of visual anomolies, or challenges with testing multiple
browsers. This all seems foreign to me because I mostly test APIs and back-end
services.

I have the luxury of ignoring problems like browser versions and changing UI
locators. However, testing APIs and services comes with its own set of challenges
that can be equally as frustrating.

SaaS are often not transactional systems. The "thing" being tested is often an
actual thing, such a virtual server, network, or load balancer. The problem with
real things is that you can't skip steps. When building a virtual server you have
to wait for the image to build the server to download, for the networking service
to provide proper IP information, and finally for the server to boot. None of these
steps can be skipped or faked in a true end-to-end test. Altogether, those steps
can take at least a few minutes, and at worst 20-30 minutes. And that's just the time
to create a single server. If you need many of these costly resources in your tests,
a test suite could easily run for multiple hours to cover a reasonable number of scenarios.

One obvious answer would be "Well don't do any of that for real! Test as much as possible with
mocks and fakes and just what you need to in a real end-to-end environment.". It's a reasonable
answer, and makes a valid point. However, there are several challenges that make it impossible to
rely too heavily on mocks:

- Developing these kinds of mocks can be expensive to develop and maintain. Systems with rules like
  networking require actual networking knowledge to mock correctly. Making mocks that just return
  something may not have the realism require to properly replace the desired component.
- There are some problems that only happen in the real world. The wonderful thing about writing code
  is that it feels ephemeral, with no rules other than the ones you create yourself. However,
  real systems have rules and boundaries. A network has limited set of IPs. A hypervisor has a limited
  number of resources it can provide. Network bandwidth and latency are real constraints. I have watched
  numerous cleverly orchestrated test suites bring test environments to their knees because they have
  no mercy on the system under test. Triggering a single action from an API can echo out across a system,
  setting off numerous workflows. Executing numerous actions in parallel can easily turn that ripple into
  a tidal wave. 