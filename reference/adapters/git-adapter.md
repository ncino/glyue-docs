# Git Adapter

The Git adapter enables Integration Gateway to perform version control operations on Git repositories. It supports common Git operations including cloning repositories, committing changes, pushing to remotes, and querying repository state.

All Git operations are executed within a sandboxed directory structure under `adapterfiles/git/` to ensure isolation and security.

## AdapterConfigGit <a href="#gitadapter-adapterconfiggit" id="gitadapter-adapterconfiggit"></a>

| Field                      | Description                                                                                                                                                                                                                                                 | Example                               |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| `Repo Base Path`           | OPTIONAL \| Relative subdirectory within the adapter's working directory (`adapterfiles/git/`) under which repositories are cloned or initialized. If not specified, repositories are managed directly in the working directory.                            | `myorg/repos`                         |
| `Default Author Name`      | OPTIONAL \| Default author name for commits. This value is used for `GIT_AUTHOR_NAME` and `GIT_COMMITTER_NAME` environment variables if not otherwise specified.                                                                                           | `Integration Gateway`                 |
| `Default Author Email`     | OPTIONAL \| Default author email for commits. This value is used for `GIT_AUTHOR_EMAIL` and `GIT_COMMITTER_EMAIL` environment variables if not otherwise specified.                                                                                        | `integrations@example.com`            |
| `Username`                 | OPTIONAL \| Username for HTTPS remote authentication. Used in conjunction with Password for authenticating with remote Git servers.                                                                                                                         | `git-user`                            |
| `Password`                 | OPTIONAL \| Password or personal access token for HTTPS remote authentication. Stored encrypted. Commonly used with GitHub, GitLab, or Bitbucket personal access tokens.                                                                                    | `ghp_xxxxxxxxxxxxxxxxxxxx`            |
| `Timeout Seconds`          | REQUIRED \| Timeout in seconds for remote Git operations (clone, fetch, pull, push). Min 1, max 300, default 300. Local operations (commit, add, status, etc.) are not subject to this timeout.                                                            | `300`                                 |

## Service Request <a href="#gitadapter-servicerequest" id="gitadapter-servicerequest"></a>

| **column**   | **value**                                                                                                                                                                                                                                                                                                                                 | **example** |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| System       | `required` The system name of this adapter                                                                                                                                                                                                                                                                                                | `GIT`       |
| Service Name | `required` The Git subcommand to execute. See [Supported Commands](#gitadapter-supportedcommands) for the full list.                                                                                                                                                                                                                      | `commit`    |

### Supported Commands <a href="#gitadapter-supportedcommands" id="gitadapter-supportedcommands"></a>

The Git adapter supports the following Git commands, organized by category:

**Repository Creation:**
- `clone` - Clone a repository from a remote URL
- `init` - Initialize a new Git repository

**File Operations:**
- `add` - Add file contents to the staging area
- `mv` - Move or rename a file, directory, or symlink
- `restore` - Restore working tree files
- `rm` - Remove files from the working tree and index

**Inspection:**
- `status` - Show the working tree status
- `log` - Show commit logs
- `show` - Show various types of objects
- `diff` - Show changes between commits, commit and working tree, etc.
- `grep` - Print lines matching a pattern
- `bisect` - Use binary search to find the commit that introduced a bug

**Branching:**
- `branch` - List, create, or delete branches
- `checkout` - Switch branches or restore working tree files
- `switch` - Switch branches
- `tag` - Create, list, delete or verify tags

**History Management:**
- `commit` - Record changes to the repository
- `rebase` - Reapply commits on top of another base tip
- `reset` - Reset current HEAD to the specified state

**Remote Operations:**
- `fetch` - Download objects and refs from another repository
- `pull` - Fetch from and integrate with another repository or local branch
- `push` - Update remote refs along with associated objects

**Configuration:**
- `config` - Get and set repository or global options

## Field Mappings <a href="#gitadapter-fieldmappings" id="gitadapter-fieldmappings"></a>

<table><thead><tr><th>Field</th><th>Description</th><th>Example</th></tr></thead><tbody><tr><td><code>repo_path</code></td><td><p>OPTIONAL for most commands | <code>str</code> | Relative path to the repository within the configured base path.</p><p>For repository-scoped commands (add, commit, push, etc.), this specifies which repository to operate on. If not provided, the adapter uses the base path.</p><p>Not applicable for <code>clone</code> (uses target from args) or <code>init</code> (uses args or defaults to base path).</p></td><td><code>'myrepo'</code></td></tr><tr><td><code>args</code></td><td><p>REQUIRED for some commands | <code>list</code> | Positional arguments for the Git command.</p><ul><li>For <code>clone</code>: First element MUST be the remote URL. Optional second element is the target directory name.</li><li>For <code>init</code>: Optional directory name to initialize.</li><li>For <code>add</code>: File paths or patterns to add (e.g., <code>['.']</code> for all files).</li><li>For <code>commit</code>, <code>branch</code>, <code>checkout</code>, etc.: Command-specific positional arguments.</li></ul></td><td><p><code>['https://github.com/user/repo.git']</code> (clone)</p><p><code>['.']</code> (add all files)</p><p><code>['main']</code> (checkout branch)</p></td></tr><tr><td><code>flags</code></td><td><p>OPTIONAL | <code>list</code> | Boolean flags to pass to the Git command.</p><p>Each element should be a complete flag string (e.g., <code>--all</code>, <code>-f</code>).</p><p>Common flags:</p><ul><li><code>--all</code> - Apply to all (context-dependent)</li><li><code>--force</code> or <code>-f</code> - Force the operation</li><li><code>--quiet</code> or <code>-q</code> - Suppress output</li><li><code>--verbose</code> or <code>-v</code> - Verbose output</li></ul></td><td><code>['--all', '--verbose']</code></td></tr><tr><td><code>kwargs</code></td><td><p>OPTIONAL | <code>dict</code> | Key-value pairs representing Git options that take values.</p><p>Each key should be the option flag (including dashes), and the value is the option's argument.</p><p>Common options:</p><ul><li><code>--message</code> or <code>-m</code> - Commit message</li><li><code>--branch</code> or <code>-b</code> - Branch name</li><li><code>--author</code> - Author name and email</li><li><code>--depth</code> - Clone depth (for shallow clones)</li></ul><p>Hyphens do not work with <code>dot.notation</code> so please use <code>['bracket-notation']</code> for keys.</p></td><td><pre><code>{
  '--message': 'Initial commit',
  '--author': 'Bot &lt;bot@example.com&gt;'
}
</code></pre><p><code>kwargs['--message'] : "Update config"</code></p><p><code>kwargs['--depth'] : "1"</code></p></td></tr><tr><td><code>env</code></td><td><p>OPTIONAL | <code>dict</code> | Additional environment variables to pass to the Git process.</p><p>Common use cases:</p><ul><li>Override <code>GIT_AUTHOR_NAME</code> / <code>GIT_AUTHOR_EMAIL</code> for specific commits</li><li>Set <code>GIT_COMMITTER_NAME</code> / <code>GIT_COMMITTER_EMAIL</code></li><li>Configure Git behavior via environment variables</li></ul><p>Note: The adapter automatically sets author/committer variables from the adapter config if provided.</p></td><td><pre><code>{
  'GIT_AUTHOR_NAME': 'Custom Author',
  'GIT_AUTHOR_EMAIL': 'custom@example.com'
}
</code></pre></td></tr></tbody></table>

## Response Format <a href="#gitadapter-responseformat" id="gitadapter-responseformat"></a>

The adapter returns a response payload with the following structure:

```python
{
    "stdout": "<command standard output>",
    "stderr": "<command standard error>",
    "status": <exit-code-integer>
}
```

- `stdout` contains the command's standard output (e.g., file listings, commit hashes, status messages)
- `stderr` contains the command's standard error (warnings, progress messages)
- `status` is the Git command exit code (0 for success, non-zero for errors)

A non-zero status code will cause the adapter to mark the response as unsuccessful.

## Service Request and Field Mapping Examples <a href="#gitadapter-servicerequestandfieldmappingexamples" id="gitadapter-servicerequestandfieldmappingexamples"></a>

### Example 1: Clone a Repository <a href="#gitadapter-example1clonearepository" id="gitadapter-example1clonearepository"></a>

Clone a repository from GitHub into the adapter's working directory.

| **System** | **Service Name** | **Formula Variable** |
| ---------- | ---------------- | -------------------- |
| GIT        | clone            | clone\_repo          |

#### Field Mappings: <a href="#gitadapter-fieldmappings1" id="gitadapter-fieldmappings1"></a>

| **Sequence** | **Field** | **Value**                                    | **Value Type** |
| ------------ | --------- | -------------------------------------------- | -------------- |
| 1            | args      | \['https://github.com/example/repo.git']     | list           |

### Example 2: Clone with Shallow Depth <a href="#gitadapter-example2clonewithshallowdepth" id="gitadapter-example2clonewithshallowdepth"></a>

Clone only the most recent commit (shallow clone) into a specific directory.

| **System** | **Service Name** | **Formula Variable** |
| ---------- | ---------------- | -------------------- |
| GIT        | clone            | clone\_shallow       |

#### Field Mappings: <a href="#gitadapter-fieldmappings2" id="gitadapter-fieldmappings2"></a>

| **Sequence** | **Field** | **Value**                                            | **Value Type** |
| ------------ | --------- | ---------------------------------------------------- | -------------- |
| 1            | args      | \['https://github.com/example/repo.git', 'myrepo']   | list           |
| 2            | kwargs    | {'--depth': '1', '--branch': 'main'}                 | dict           |

### Example 3: Check Repository Status <a href="#gitadapter-example3checkrepositorystatus" id="gitadapter-example3checkrepositorystatus"></a>

Check the working tree status of a repository.

| **System** | **Service Name** | **Formula Variable** |
| ---------- | ---------------- | -------------------- |
| GIT        | status           | check\_status        |

#### Field Mappings: <a href="#gitadapter-fieldmappings3" id="gitadapter-fieldmappings3"></a>

| **Sequence** | **Field**   | **Value** | **Value Type** |
| ------------ | ----------- | --------- | -------------- |
| 1            | repo\_path  | 'myrepo'  | str            |

### Example 4: Add Files and Commit <a href="#gitadapter-example4addfilesandcommit" id="gitadapter-example4addfilesandcommit"></a>

Stage all changes and create a commit with a message.

#### Service Request 1 - Add Files:

| **System** | **Service Name** | **Formula Variable** |
| ---------- | ---------------- | -------------------- |
| GIT        | add              | stage\_files         |

#### Field Mappings: <a href="#gitadapter-fieldmappings4a" id="gitadapter-fieldmappings4a"></a>

| **Sequence** | **Field**  | **Value** | **Value Type** |
| ------------ | ---------- | --------- | -------------- |
| 1            | repo\_path | 'myrepo'  | str            |
| 2            | args       | \['.']    | list           |

#### Service Request 2 - Commit:

| **System** | **Service Name** | **Formula Variable** |
| ---------- | ---------------- | -------------------- |
| GIT        | commit           | create\_commit       |

#### Field Mappings: <a href="#gitadapter-fieldmappings4b" id="gitadapter-fieldmappings4b"></a>

| **Sequence** | **Field**  | **Value**                                      | **Value Type** |
| ------------ | ---------- | ---------------------------------------------- | -------------- |
| 1            | repo\_path | 'myrepo'                                       | str            |
| 2            | kwargs     | {'--message': 'Automated update from Gateway'} | dict           |

### Example 5: Push Changes to Remote <a href="#gitadapter-example5pushchangestoremote" id="gitadapter-example5pushchangestoremote"></a>

Push committed changes to a remote repository. Requires Username and Password configured in the Adapter Config.

| **System** | **Service Name** | **Formula Variable** |
| ---------- | ---------------- | -------------------- |
| GIT        | push             | push\_changes        |

#### Field Mappings: <a href="#gitadapter-fieldmappings5" id="gitadapter-fieldmappings5"></a>

| **Sequence** | **Field**  | **Value** | **Value Type** |
| ------------ | ---------- | --------- | -------------- |
| 1            | repo\_path | 'myrepo'  | str            |
| 2            | args       | \['origin', 'main'] | list           |

### Example 6: Create a New Branch <a href="#gitadapter-example6createanewbranch" id="gitadapter-example6createanewbranch"></a>

Create and checkout a new branch.

| **System** | **Service Name** | **Formula Variable** |
| ---------- | ---------------- | -------------------- |
| GIT        | checkout         | new\_branch          |

#### Field Mappings: <a href="#gitadapter-fieldmappings6" id="gitadapter-fieldmappings6"></a>

| **Sequence** | **Field**  | **Value**            | **Value Type** |
| ------------ | ---------- | -------------------- | -------------- |
| 1            | repo\_path | 'myrepo'             | str            |
| 2            | flags      | \['-b']              | list           |
| 3            | args       | \['feature-branch']  | list           |

### Example 7: Configure Repository <a href="#gitadapter-example7configurerepository" id="gitadapter-example7configurerepository"></a>

Set repository-level configuration (e.g., user name for commits).

| **System** | **Service Name** | **Formula Variable** |
| ---------- | ---------------- | -------------------- |
| GIT        | config           | set\_config          |

#### Field Mappings: <a href="#gitadapter-fieldmappings7" id="gitadapter-fieldmappings7"></a>

| **Sequence** | **Field**  | **Value**                | **Value Type** |
| ------------ | ---------- | ------------------------ | -------------- |
| 1            | repo\_path | 'myrepo'                 | str            |
| 2            | args       | \['user.name', 'Bot User'] | list           |

### Example 8: Pull Latest Changes <a href="#gitadapter-example8pulllatestchanges" id="gitadapter-example8pulllatestchanges"></a>

Pull the latest changes from the remote repository.

| **System** | **Service Name** | **Formula Variable** |
| ---------- | ---------------- | -------------------- |
| GIT        | pull             | pull\_updates        |

#### Field Mappings: <a href="#gitadapter-fieldmappings8" id="gitadapter-fieldmappings8"></a>

| **Sequence** | **Field**  | **Value** | **Value Type** |
| ------------ | ---------- | --------- | -------------- |
| 1            | repo\_path | 'myrepo'  | str            |
| 2            | args       | \['origin', 'main'] | list           |

## Security Considerations <a href="#gitadapter-securityconsiderations" id="gitadapter-securityconsiderations"></a>

- **Credential Storage**: Remote authentication credentials (Username/Password) are stored encrypted in the Adapter Config. Use personal access tokens rather than passwords when possible.
- **Sandboxed Operations**: All Git operations are confined to the `adapterfiles/git/` directory structure to prevent unauthorized filesystem access.
- **HTTPS Only**: The adapter supports HTTPS authentication for remote operations. SSH authentication is not currently supported.
- **Timeouts**: Remote operations are subject to configurable timeouts to prevent indefinite hangs on network issues.

## Additional Notes <a href="#gitadapter-additionalnotes" id="gitadapter-additionalnotes"></a>

- The Git binary must be installed and available on the system PATH for the adapter to function.
- Repository paths are always relative to the adapter's working directory (`<BASE_DIR>/adapterfiles/git/` plus optional `repo_base_path`).
- The adapter uses GitPython internally but executes the actual `git` command-line binary for all operations.
- Commit author and committer identity can be set at the adapter config level (applies to all operations) or overridden per-request via the `env` field mapping.
- The adapter automatically creates necessary directory structures when cloning or initializing repositories.
