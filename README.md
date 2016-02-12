# ![alt text](https://avatars2.githubusercontent.com/u/1263946?v=3&s=84 "KBase") KBase SDK

**This software is still in beta and should only be used for internal KBase development.**

The KBase SDK is a set of tools for developing new modules that can be dynamically registered and run on the KBase platform.  Modules include all code, specification files, and documentation needed to define and run a set of methods in the KBase Narrative interface.

There are a number of restrictions on module functionality that will gradually be lifted as the SDK and KBase platform are refined.  The current set of restrictions are:

- Runs on a standard KBase worker node (at least 2 cores and 22GB memory)
- Operates only on supported KBase data types
- Requires either no or fairly limited amounts of reference data
- Uses existing data visualization widgets
- Does not require new uploaders/downloaders
- Wrapper written in Python, Java, or Perl
 
In order to register your SDK module, you have to be an approved KBase developer.  To become an approved KBase developer, first create a standard KBase user account through http://kbase.us.  Once you have an account, please contact us with your username and we will help with the next steps.


## <A NAME="steps"></A>Steps in Using SDK
1. [Install SDK Dependencies](doc/kb_sdk_dependencies.md)
2. [Install and Build SDK](doc/kb_sdk_install_and_build.md)
3. [Create Module](doc/kb_sdk_create_module.md)
4. [Edit Module and Method(s)](doc/kb_sdk_edit_module.md)
5. [Locally Test Module and Method(s)](doc/kb_sdk_local_test_module.md)
6. [Register Module](doc/kb_sdk_register_module.md)
7. [Test in KBase](doc/kb_sdk_test_in_kbase.md)
8. [Complete Module Info](doc/kb_sdk_complete_module_info.md)
9. [Deploy](doc/kb_sdk_deploy.md)

### Additional Documentation
- [FAQ](doc/FAQ.md)
- Troubleshooting
- [Examples](#examples)
- KBase Developer Policies
- KBase SDK Coding Style Guide and Best Practices
- [Anatomy of a KBase Module](doc/module_overview.md)
- [KBase Data Types Table](doc/kb_sdk_data_types_table.md)
- [Working with KBase Data Types](doc/kb_sdk_data_types.md)
- Wrapping an Existing Command-Line Tool
- Using Custom Reference Data 
- [kb-sdk Command Line Interface](doc/Module_builder.md) (NEEDS UPDATING)
- (Combine [Module Testing Framework](doc/testing.md) with [Debugging Your Module](doc/Docker_deployment.md))
- Managing Your Module Release Cycle
- KBase Interface Description Language (KIDL) Guide
- Visualization Widget Development Guide
- [KBase Catalog API](https://github.com/kbase/catalog/blob/master/catalog.spec)

### Quick Install Guide

Below is a quick reference guide for installation.  For more complete details and troubleshooting, see the [Full Installation Guide](doc/installation.md).

#### Installation Only

System Dependencies:

- Mac OS X 10.8+ or Linux
- Java JRE 7+ http://www.oracle.com/technetwork/java/javase/downloads/index.html
- (Mac only) Xcode https://developer.apple.com/xcode
- git https://git-scm.com
- Docker https://www.docker.com (for local testing)

Get the SDK:

    git clone https://github.com/kbase/kb_sdk

Pull dependencies and configure the SDK:

    cd kb_sdk
    make bin

Download the local KBase SDK base Docker image:

    make sdkbase 

Add the kb-sdk tool to your PATH and enable command completion.  From the kb_sdk directory:

    # for bash
    export PATH=$(pwd)/bin:$PATH
    source src/sh/sdk-completion.sh

Test installation:

    kb-sdk help


#### Build from source

Additional System Dependencies:

- Java JDK 7+ http://www.oracle.com/technetwork/java/javase/downloads/index.html
- JAVA_HOME environment variable set to JDK installation path
- Apache Ant http://ant.apache.org

Follow basic instructions above.  Instead of running `make bin` you can run `make` to compile the SDK:

    cd kb_sdk
    make


### Quick Start Guide

Initialize a new module populated with the ContigCount example (module names need to be unique in KBase, so you should pick a different name):

    kb-sdk init --example -l python -u [your_kbase_user_name] ContigCount

Enter your new module directory and do the initial build:

    cd ContigCount
    make

Edit the local test config file (`test_local/test.cfg`) with a KBase user account name and password:

    test_user = TEST_USER_NAME
    test_password = TEST_PASSWORD

Run tests:

    cd test_local
    kb-sdk test

This will build your Docker container, run the method implementation running in the Docker container that fetches example ContigSet data from the KBase CI database and generates output.

Inspect the Docker container by dropping into a bash console and poke around, from the `test_local` directory:
    
    ./run_bash.sh

When you make changes to the Narrative method specifications, you can validate them for syntax locally.  From the base directory of your module:

    kb-sdk validate

Add your repo to [GitHub](http://github.com) (or any other public git repository), from the ContigCount base directory:

    cd ContigCount
    git init
    git add .
    git commit -m 'initial commit'
    # go to github and create a new repo that is not initialized
    git remote add origin https://github.com/[GITHUB_USER_NAME]/[GITHUB_REPO_NAME].git
    git push -u origin master

Go to https://narrative-ci.kbase.us and start a new Narrative.  Search for the SDK Register Repo method, and click on it.  Enter your public git repo url (e.g. https://github.com/[GITHUB_USER_NAME]/[GITHUB_REPO_NAME]) and register your repo.  Wait for the registration to complete.  Note that you must be an approved developer to register a new module.

Click on the 'R' in the method panel list until it switches to 'D' for methods still in development.  Find your new method by searching for your module, and run it to count some contigs.

Now, dive into [Making your own Module](doc/kb_sdk_dependencies.md).

Please email us when you think your module is ready for public use.


### <A NAME="examples"></A>Example Modules

There are a number of modules that we continually update and modify to demonstrate best practices in code and documentation and present working examples of how to interact with the KBase API and data models.

 - [MegaHit](https://github.com/msneddon/kb_megahit) (Python) - assembles short metagenomic read data (Read Data -> Contigs)
 - [Trimmomatic](https://github.com/psdehal/Trimmomatic) (Python) - filters/trims short read data (Read Data -> Read Data)
 - [OrthoMCL](https://github.com/rsutormin/PangenomeOrthomcl) (Python) - identifies orthologous groups of protein sequences from a set of genomes (Annotated Genomes / GenomeSet -> Pangenome)
 - [ContigFilter](https://github.com/msneddon/ContigFilter) (Python) - filters contigs based on length (ContigSet -> ContigSet)


### Need more?

If you have questions or comments, please create a GitHub issue or pull request, or contact us through http://kbase.us
