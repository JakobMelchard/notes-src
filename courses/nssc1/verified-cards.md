# nssc1 (verified cards)

Cards kept after the 2026W check against the downloaded lecture material. Unverified cards and prose were removed.

## 04-finite-difference-discretisation

```card id=nssc-04-poisson1d
Give the 1D Poisson FD matrix and how Dirichlet values enter.
---
$A = \frac{1}{h^2}\,\text{tridiag}(-1, 2, -1)$ of size $N-1$.
Boundary values move to $b$: $b_1 \mathrel{+}= \alpha/h^2$, $b_{N-1} \mathrel{+}= \beta/h^2$.
```

```card id=nssc-04-five-point
Give the 2D five-point stencil and the matrix structure under lexicographic ordering.
---
$\frac{4u_{i,j} - u_{i\pm1,j} - u_{i,j\pm1}}{h^2} = f_{i,j}$.
Block tridiagonal $A_2 = I \otimes A_1 + A_1 \otimes I$, 5 nnz per row, bandwidth $N-1$.
```

```card id=nssc-04-helmholtz
How does the Helmholtz equation $\nabla^2\phi \pm k^2\phi = b$ change the discrete FD matrix compared with Poisson?
---
The sparsity pattern stays the same; the diagonal shifts by $\pm k^2$, so $-2/h^2$ becomes $-2/h^2 \pm k^2$.
```

## 05-numerical-linear-algebra

```card id=nssc-05-csr
Describe the three CSR arrays.
---
`val[nnz]` and `col[nnz]` list nonzeros row by row.
`row_ptr[n+1]` gives each row start, `row_ptr[n] = nnz`.
```

```card id=nssc-05-csr-example
Write the CSR arrays of $\begin{pmatrix} 4 & -1 & 0 \\ -1 & 4 & -1 \\ 0 & -1 & 4 \end{pmatrix}$.
---
`val = [4,-1,-1,4,-1,-1,4]`, `col = [0,1,0,1,2,1,2]`, `row_ptr = [0,2,5,7]`.
```

```card id=nssc-05-storage-formats
Name the three sparse storage formats covered in the lecture.
---
COO (coordinate list of row, column and value), CSR (compressed sparse row), CSC (compressed sparse column).
```

```card id=nssc-05-fill-in
Why is the inverse of a sparse matrix a poor route to solving $Ax = b$?
---
The inverse of a sparse matrix, even a tridiagonal one, is generally dense.
Storing it costs $N^2$ entries, and products with it are expensive.
```

```card id=nssc-05-no-iterative
Name two cases where a non-iterative solver is enough.
---
1D tridiagonal systems with the Thomas algorithm, which is efficient and not iterative.
Time-dependent N-D problems split into 1D problems, each solved with Thomas, and banded matrices with a banded direct solver.
```

```card id=nssc-05-tolerance-halfway
Why can an iterative method use a looser tolerance inside a larger algorithm, while a direct method cannot?
---
An iterative method can stop at any accuracy. If an earlier step adds error of order $10^{-3}$, the later iterative step only needs a tolerance around $10^{-4}$ to be on the safe side.
Direct methods cannot operate half-way.
```

```card id=nssc-05-warm-start
What initial guess should an iterative solve use inside a time-marching loop?
---
The solution of the previous time step (warm start).
```

```card id=nssc-05-stop-tests
Name the three stopping tests of the iteration procedure.
---
$N_{\max}$ caps the iteration count; reaching it is an error.
Residual test $\|r\| < \text{tol}_r$ with $r = Ax - b$; step test $\|x - x_{\text{old}}\| < \text{tol}_x$, which warns that the iterates barely change.
Whether to normalise by $\|b\|$ is problem dependent.
```

```card id=nssc-05-splitting-form
Write the stationary iteration for $A = M - N$ and the requirement on $M$.
---
$x^{k+1} = x^k + M^{-1} r^k$ with $r^k = b - Ax^k$, equivalently $M x^{k+1} = b + N x^k$.
$M$ should be as close to $A$ as possible and cheap to invert.
```

```card id=nssc-05-jacobi-lecture
What does the lecture say about the convergence of Jacobi, and why is it taught?
---
Probably the worst convergence rate of the basic methods, but convergence is still guaranteed.
It is the building block (smoother) inside multigrid.
```

```card id=nssc-05-stall
Why does a stationary iteration stall, and what does multigrid do about it?
---
After a few iterations only the smooth part of the error is left, and it converges very slowly.
Multigrid moves the problem to a coarser grid, where the smooth error looks rapidly varying and is removed cheaply, then interpolates back.
```

```card id=nssc-05-error-residual
How are error and residual related, and how is a solver's performance measured?
---
$A e^{(m)} = r^{(m)}$ with $e^{(m)} = x - x^{(m)}$, so the error goes to zero as the residual does.
Performance is measured by the number of iterations needed to drive the residual to zero.
```

```card id=nssc-05-restrict-prolong
What are restriction and prolongation in the multigrid scheme?
---
Restriction moves the residual to the coarse grid, in the lecture example by taking every other point.
Prolongation moves the coarse correction back to the fine grid by linear interpolation.
```

```card id=nssc-05-vw-cycle
Describe the V cycle and the W cycle.
---
A V cycle goes from the finest grid directly to the coarsest and back, and repeats until convergence.
A W cycle does not go directly; it visits the coarse grids more often.
```

```card id=nssc-05-mg-example
In the lecture example (Gauss-Seidel per level, six levels from N=64 to N=2), how many V cycles bring the residual below $10^{-3}$, and how many Gauss-Seidel iterations are needed without multigrid?
---
8 V cycles; plain Gauss-Seidel needs 2580 iterations.
```

```card id=nssc-05-matrix-free
What must a matrix-free Jacobi solver store, and what may it not allocate?
---
Only the solution vectors. Matrix entries come from the stencil computed from grid indices, with boundary points handled as in the COO assembly.
No new, malloc or standard containers inside the solver, and no static arrays that scale with the problem size.
```

```card id=nssc-05-exact-solution
What is the exact solution of the 2D Laplace test problem of the Jacobi exercise, with data $\sin \pi x$ on the top side and zero on the other sides?
---
$f(x, y) = \dfrac{\sinh(\pi y)\,\sin(\pi x)}{\sinh \pi}$ on the unit square.
```

```card id=nssc-05-flags
Can compiler optimisation flags change the result of a Jacobi run?
---
Yes, in general: reordering floating-point operations, for example by vectorisation, changes rounding, so bitwise reproducibility can be lost.
Weigh the time gain against that loss.
```

## 10-software-engineering-for-scientific-computing

```card id=nssc-se-practices
Name four software development practices and the problem each addresses.
---
Version control (it worked half a year ago), documentation and build automation (how do you build this?), testing and CI (did my change break something?), isolation with containers or VMs (installing tools broke my other project). Also code review, IaC, modularity.
```

```card id=nssc-se-vm-vs-container
Virtual machine vs container.
---
A VM runs a guest OS on virtual hardware provided by the host (hardware support reduces the overhead). A container shares the host kernel and is isolated by OS features such as cgroups: lighter, faster, the usual choice for reproducible environments.
```

```card id=nssc-se-meta-build
What is a meta-build tool, with examples?
---
A tool that generates the input for a build tool (Make, Ninja) and detects compilers, finds dependencies, handles platforms, testing and packaging. Examples: CMake, Meson, autotools.
```

```card id=nssc-se-interop
Three ways to call code across languages.
---
Foreign function interface (ctypes, `extern "C"`), language bindings that wrap an API (pybind11, nanobind, Boost.Python), and embedding an interpreter in a binary (Python inside C++). Lower-level ways usually perform better.
```

```card id=nssc-se-coding-agent
What makes an AI coding assistant an "agent"?
---
An LLM coupled to tools (read and edit files, run commands, fetch resources) in a feedback loop, with the context holding user input, files and history.
```

```card id=nssc-se-git-states
States of a file in a git working tree.
---
Untracked (unknown to git), modified (changed since the last commit), staged (in the index, will go into the next commit), committed (stored in the repository). HEAD marks the commit you compare against and build on.
```

```card id=nssc-se-merge
How does git merge two branches?
---
Find the common ancestor, compute each branch's changes since then, combine them in a merge commit with both tips as parents. Changes to the same lines conflict and need manual resolution. If only one branch moved, it is a fast-forward and no merge commit is made.
```

```card id=nssc-se-fetch-pull
`git fetch` vs `git pull`, and what `pull --rebase` changes.
---
Fetch downloads the remote branch's objects and head. Pull = fetch + merge. With `--rebase` your local commits are replayed on top of the upstream instead, avoiding a merge commit.
```

```card id=nssc-se-history-rule
The one rule about rewriting git history (amend, rebase).
---
Never rewrite history you have already shared: everyone who built on it would have to resolve the conflicts again.
```

```card id=nssc-se-docker-layers
Dockerfile: what creates a layer, and how do you keep images small?
---
Each `RUN` or `COPY` instruction makes a new cached layer. Install and clean up in the same `RUN`, since files deleted in a later layer still take space in the earlier one.
```

```card id=nssc-se-ci-pipeline
GitLab CI/CD: pipeline, job, runner.
---
A pipeline (YAML in the repo) is a set of jobs with dependencies, in stages or as a DAG, triggered e.g. per commit. A job is a list of commands in a defined environment; runners execute jobs (shell, Docker, VM, Kubernetes executors). Artifacts pass files to later jobs, caches reuse data across pipelines.
```

```card id=nssc-se-cmake-vars
CMake: which names are case sensitive, and what is a cache variable?
---
Commands are case insensitive, variables are case sensitive. Cache variables hold user input, live in `CMakeCache.txt` and are set with `cmake -D VAR=value`; a normal variable of the same name can hide them.
```

```card id=nssc-se-out-of-source
Why use out-of-source builds in CMake?
---
Generated files stay out of the source tree, and one source tree can have several build directories with different configurations (debug, release, other compilers).
```

```card id=nssc-se-four-freedoms
The FSF's four freedoms of free software.
---
0 run the program for any purpose; 1 study and change it; 2 redistribute copies; 3 distribute modified versions. "Free as in speech, not as in beer": free software may be sold.
```

```card id=nssc-se-licence-types
Proprietary, copyleft, permissive: what distinguishes them?
---
Proprietary: object code only, source a trade secret. Copyleft (GPL, LGPL, MPL): derived works must be distributed under the same terms. Permissive (MIT, BSD, Apache): may be included in closed software, requires attribution and keeps the warranty disclaimer.
```

```card id=nssc-se-gpl-trigger
When do the GPL's obligations apply?
---
On distribution. Using or modifying GPL code inside an organisation without distributing it creates no obligation; distributing a combined work requires GPL with source. The LGPL relaxes this for dynamic linking.
```

```card id=nssc-se-no-licence
What may you do with published code that carries no licence?
---
Almost nothing beyond reading it: copyright applies automatically at creation, so without a licence you have no right to copy, modify or redistribute.
```

```card id=nssc-se-makefile
What does a Makefile describe?
---
Targets with prerequisites and commands, forming a dependency graph from the inputs (sources) to the output (an executable).
Make reads hand-written Makefiles; Ninja files are usually machine generated.
```

```card id=nssc-se-build-tools
How do build automation tools and build configuration generators relate?
---
Build automation tools (make, Ninja) run the dependency graph from sources to executable.
Configuration generators (CMake, Meson, autotools) write that input and also detect compilers, find dependencies and handle platforms.
```

```card id=nssc-se-cmake-tests-packages
How are tests and packages defined and built with CMake?
---
`enable_testing()` and `add_test(...)` define tests, and `ctest` runs them (`-R` filters by name, `-VV` is verbose).
`install(...)` rules feed CPack, which generates installers and source packages such as RPM, DEB or archives.
```

```card id=nssc-se-cmake-target
What is a CMake target?
---
An executable, library or utility that the build tool knows about.
`add_library(name source.cpp)` makes the library usable across the project, and dependencies between targets are declared.
```

```card id=nssc-se-cmake-generator
How do you choose the build tool that CMake generates input for?
---
With `-G`, for example `cmake -G Ninja` or `cmake -G "Unix Makefiles"`, or with the environment variable `CMAKE_GENERATOR`.
`cmake --build` then invokes the chosen tool.
```

```card id=nssc-se-cmake-practice
Name three CMake good practices from the course.
---
Out-of-source builds; `cmake_minimum_required(VERSION 3.15...4.0)` with the lowest supported and the highest tested version; stable project and target names written as literals, not variables.
```

```card id=nssc-se-git-object
What is a git object, and how is it identified?
---
Everything git stores (files, commits, tags) is an object in `.git`, named by the SHA-1 hash of its content, so an object cannot be changed in place.
```

```card id=nssc-se-git-commit-steps
What are the two steps of a git commit?
---
Add files to the index (staging area) to mark them for the next commit, then commit all staged files with a message.
Commit often, by topic, and do not mix unrelated changes.
```

```card id=nssc-se-gitignore
What does a `.gitignore` file do?
---
Ignored paths are not shown as untracked and are not added by `git add`. Patterns allow wildcards, and `!` negates a pattern.
Track the `.gitignore` itself, and use it for generated files, binaries and large data.
```

```card id=nssc-se-git-tools
Which git commands compare commits, show who changed a line, and undo a commit?
---
`git diff` shows the differences between commits or files, `git blame` shows who changed each line of a file and when, and `git revert` undoes the changes of a past commit.
```

```card id=nssc-se-git-submodule
What is a git submodule?
---
A separate repository inside another repository. The parent project tracks only the commit SHA of the submodule, which pins the library's version.
```

```card id=nssc-se-git-stash
What is `git stash` for?
---
It stores uncommitted modifications so you can switch branches or pull; they come back with `git stash apply`.
The stash is a stack, so several layers of uncommitted work can be kept.
```

```card id=nssc-se-docker-image
What is the difference between a Docker image and a container, and what is a build context?
---
An image is a read-only template built from a Dockerfile; a container is a runnable instance of an image.
The build context is the set of files available to the build.
```

```card id=nssc-se-doc-gen
Name three documentation generators and the languages they serve.
---
Doxygen, mainly for C and C++; MkDocs, written in and for Python; Sphinx, written in Python and widely used across languages.
```

```card id=nssc-se-ci-rules
What do `workflow` and job `rules` decide in GitLab CI/CD?
---
`workflow` decides which pipelines are created at all. Job `rules` decide when an individual job is added to a pipeline.
Job names that start with a dot are templates and are never added to a pipeline.
```

```card id=nssc-se-agent-limits
What strengths and limits of coding agents does the lecture give?
---
The lecturer's stated opinion, not a scientific fact. Strengths: simple, tedious tasks, finding inconsistencies between docs and code, exploring large codebases, planning implementations.
Limits: large architectural changes, long-running tasks, open-ended tasks done independently, and unclear instructions.
```

```card id=nssc-se-oss-vs-free
Is open source the same as free software?
---
No. Free refers to freedom, not price; the FSF defines it through four freedoms. Free software can be commercial.
Open source is a development model and a licence category, defined separately by the OSI with its own criteria.
```

```card id=nssc-se-copyright-rights
Which rights does copyright grant, and what is protected?
---
Economic rights: reproduction, public display and making adaptations. Moral rights: to be recognised as the author and to protect the work's integrity.
Copyright arises automatically on creation. An idea is not protected, only its expression.
```

```card id=nssc-se-public-domain
What do public domain and Creative Commons mean?
---
Public domain: copyright has been removed, so the work can be shared, modified and even made proprietary.
Creative Commons: licences inspired by open source, intended mainly for non-software works such as text, music and art.
```

```card id=nssc-se-fsf-osi
What do the FSF and the OSI each do?
---
The FSF (founded 1985 by Richard Stallman) sponsors GNU, publishes the GPL and campaigns for free software.
The OSI (founded 1998) stewards the Open Source Definition and certifies open source licences.
```
