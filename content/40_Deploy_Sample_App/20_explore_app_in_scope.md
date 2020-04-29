+++
title = "Explore App in Scope"
chapter = false
weight = 20
+++

## Validate the installation

In Scope:

Explore the <b>Technologies</b> view.

- This shows the containerized processes running on your nodes, grouped by technology (runtime, framework, vendor)
- Clicking on a technology display the <b>Process Groups</b> for this technology
- You can drill down to individual process instance
  
Look at the <b>Transactions & services</b> view.

- No <b>Service</b> will show up
- This is because the underlying processes are instrumented at startup by the <b>OneAgent</b>
- Because those processes/containers were already running before we deployed the <b>OneAgent</b>, they need to be restarted to get instrumented
