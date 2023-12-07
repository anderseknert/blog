Title: December hacks
Date: 2023-12-07
Image: assets/images/regal.jpg
Image-Alt: Regal the Rego linter, logo picturing the Regal viking
Tags: code, open source, regal, mcov, tools

Some great stuff happening in and around [Regal](https://docs.styra.com/regal) at the moment. First, [v0.14.0](https://github.com/StyraInc/regal/releases/tag/v0.14.0) was released earlier this week, and it's quite a solid one. Two new linter rules, a new output format, and a bunch of fixes and improvements in existing rules. The new output format is [SARIF](https://sarifweb.azurewebsites.net/), which is a standardized format tools like linters can use to report their findings. The format seems well-supported by a number of tools, but I've yet only tried it out using the [SARIF VS Code extension](https://github.com/microsoft/sarif-vscode-extension).

<img src="assets/images/regal.jpg" alt="Regal" title="Regal the Rego linter" width="300"/>

Next, we're running a [holiday campaign](https://github.com/orgs/open-policy-agent/discussions/534) around Regal, where we'll be giving away a few custom linter rules as gifts to the community every now and then during December. And not just rules, but some fun and hidden features in Regal too. It's not every day you get a chance to work on silly or fun stuff like this, and I find I really enjoy it. There's a few seriously cool rules we've been working on for this too though, and my colleague [Charlie](https://github.com/charlieegan3) delivered one of the cooler rules I've seen in any linter. More on that in a follow-up post!

### Minimum Compatible OPA Version (mcov)

A few weeks ago, [Torin Sandall](https://github.com/tsandall) made some changes in OPA to allow the compiler to keep track of what capabilities (i.e. features such as built-in functions) that's been used as part of policy compilation. This information may then be used to determine what the minimum compatible version of OPA is required to evaluate the policy. Having access to that through the Go API is obviously enough if the purpose is to integrate that feature in tools working with OPA and Rego. However...

I tend to do a lot of changes in policy as a result of linting with Regal, and one thing I'd like to check is how these changes affect the minimum compatible version of OPA required. I could probably integrate something for that in Regal, but given that there are other relevant use cases for this, I figured a standalone tool would be a better option. This tool is [mcov](https://github.com/StyraInc/mcov), and along with the "OPA versions cheatsheet" that I wrote to keep track of what changes happened in any given OPA release, it's a rather nice little utility.
