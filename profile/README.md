<p align="center">
  <img src="logo.jpg" alt="Limulus logo" width="200" />
</p>

<h1 align="center">Limulus</h1>

<p align="center">
  <strong>One experiment. Many universes.</strong><br />
  A visual Python workspace for exploring every “what if.”
</p>

<p align="center">
  <a href="#the-idea">The idea</a> ·
  <a href="#how-universes-work">Universes</a> ·
  <a href="#the-project">The project</a> ·
  <a href="#get-started">Get started</a>
</p>

---

> **Active development:** Limulus is under active development, and breaking changes are still occurring.
> Features, APIs, and file formats may change between versions.
> **We welcome your feedback!** [Report a bug, suggest a feature, or share your experience](https://github.com/LimulusGraph/clients/issues).

Branch a data pipeline, try a different model, and follow both ideas to their
results. Limulus is a node-based alternative to notebooks: your Python files form
a graph, and every complete path through that graph becomes its own experiment.

## The idea

Experiments rarely follow a straight line. You might want to compare two
preprocessing steps, swap a training configuration, or reuse an evaluation script
across several models. Limulus makes those alternatives visible in the graph,
without requiring a copy of every file or a Git branch for every variation.

| Build once | Explore alternatives | Keep your code |
| --- | --- | --- |
| Put reusable steps in ordinary Python files and connect them as nodes. | Branch into independent execution paths, called **universes**, and run them concurrently. | Store the graph separately from source, so code stays readable, editable, and ready for Git. |

A node points to a `.py` file. A `.lmg` JSON file records the graph and its
connections. Runtime objects stay in Python memory; the graph stores structure.

## How universes work

Every path from a node with no enabled inputs to a node with no enabled outputs
is a separate universe. A standalone node is a universe too.

```text
                  ┌─ Model A ─┐
Load → Prepare ────┤           ├── Evaluate
                  └─ Model B ─┘

Universe 1: Load → Prepare → Model A → Evaluate
Universe 2: Load → Prepare → Model B → Evaluate
```

Each universe owns a persistent Python process. Nodes execute in order within
that process, sharing live variables, imports, functions, classes, and objects.
Different universes run concurrently with independent Python state.

In this example, both paths use the same loading, preparation, and evaluation
source files, but execute their own copies. Evaluate sees the model from its own
path; the converging connections do not merge the two runtimes.

Run a complete experiment, restart a path through a selected node, or rerun one
node in an existing runtime. Use a fresh run after changing upstream code when
you want downstream results to reflect that change.

Python memory is isolated, while files, network services, and accelerators are
shared. Shared steps execute once per universe, including their external side
effects. Runtime state lasts for the session; source and graph files are the
persistent project representation.

## The project

Limulus is organized around a server and a VS Code extension:

| Repository | Role |
| --- | --- |
| [VS Code extension](https://github.com/LimulusGraph/vscode-extension) | The integrated editor experience: graph editing, Python source navigation, Markdown and rich output previews, and project-local pip environments. |
| [Server](https://github.com/LimulusGraph/server) | Hosts the local browser interface and Python execution backend. Validates paths, manages persistent Python processes, and streams results. |

The browser workflow follows the local-server model familiar from Jupyter: start
Limulus on your machine and open its graph interface in a web browser. The VS Code
extension brings the same graph-and-Python model into the editor, with source
navigation, previews, and automatic dependency management.

## Get started

**Inside VS Code:** follow the
[extension installation guide](https://github.com/LimulusGraph/vscode-extension#requirements-and-installation).
The extension package includes the backend. Create a graph, add nodes, edit their
Python files, and run your first universe without leaving the editor.

**In a web browser:** start the Limulus server locally and open its graph
interface in your browser, just as you would with Jupyter. Follow the
[server setup guide](https://github.com/LimulusGraph/server#running) for
installation and launch instructions.

**Explore the runtime:** the
[sample universe project](https://github.com/LimulusGraph/server/tree/main/examples/universes)
demonstrates two paths sharing source files while retaining separate models and
execution state. The [runtime guide](https://github.com/LimulusGraph/server#architecture-and-api)
describes the process model and streaming API.

The Python execution backend currently targets macOS and Linux. For component
requirements, setup, and development instructions, use the corresponding repository.

## Get involved

Try Limulus with your own pipeline, share a reproducible experiment, or help
improve an interface or the runtime. Report issues and propose changes in the
repository responsible for the behavior, including a small graph and Python
example when possible.
