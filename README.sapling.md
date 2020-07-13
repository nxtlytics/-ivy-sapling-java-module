# Java Module Sapling

This repository contains a sapling (skeleton) for building a Java Module using Maven that can be uploaded to Maven.

Modules uploaded to Maven can be used by other modules or Java applications using the Java Application sapling.

## Getting started

* Click "Use this template", and name your new creation.  
    
  ![you know it](https://help.github.com/assets/images/help/repository/use-this-template-button.png)

* Clone the resulting project to your computer
  ```
  cd ~/code
  git clone git@github.com:your-org/my-module.git
  ```

* Remove this `README.md` file, and replace it with your own

* Edit the `pom.xml`
  * Update the `groupId` and `artifactId` sections
  * Add your dependencies

* Add your code
  * Create a folder with the same name as your module
  * Run `mvn compile` to sync your local dependencies  
    *Dependencies installed, but not listed in your lock file will not be installed in CircleCi!*
  
* Commit your code
  * See the section on [Workflow](#Workflow) for Git workflow processes and best practices

* Add your project to CircleCi

* Party!

## Using modules

### Maven Naming Conventions

See [maven here](https://maven.apache.org/guides/mini/guide-naming-conventions.html) and see [java here](https://docs.oracle.com/javase/specs/jls/se6/html/packages.html#7.7)

### Production modules
If you've created a project called `yodelling_coach` and had just freshly minted release `0.9.9`,  
your artifact would be called `yodelling_coach` version `0.9.9`.

Using this package in another `pom.xml` would look like:
```
...
    <dependency>
      <groupId>com.nxtlytics.team</groupId>
      <artifactId>com.nxtlytics.team.yodelling_coach</artifactId>
      <version>0.0.9</version>
    </dependency>
...
```

### Prerelease modules
If you've created a project called `awesome_sauce` and were working off snapshot `0.7.4-SNAPSHOT` in the `develop` branch,
your artifact would be called `awesome_sauce` version `0.7.4-SNAPSHOT`.

This would be considered a **prerelease** module by Maven, so to reference it in a `pom.xml` of another project, you would need to
reference it with it's exact name.
```
...
    <dependency>
      <groupId>com.nxtlytics.team</groupId>
      <artifactId>com.nxtlytics.team.awesome_sauce</artifactId>
      <version>0.7.4-SNAPSHOT</version>
    </dependency>
...
```

## Workflow

### Git bootstrap

The initial Git bootstrap workflow is:

1. Clone the sapling
2. Add your code in any state (working or not)
3. Commit your code
4. Update `pom.xml` to reflect the version of the tag you are about to create.
5. Tag the `master` branch with a semver (`0.0.0` for initial commit is suggested)

CircleCi will build a `0.0.0` version (based on the version declared in `pom.xml`) of your library which depending on the state of your initial code commit may be all you need.

### Collaborating and Developing

Once you've decided you need to make updates or fix bugs, you've got two options:
* `feature-*` branches -> `develop` branch -> `master` branch  
  Have multiple contributors? Want separation of duties and potentially a benevolent merge-dictator?  
  Use this method if so.
  
* `develop` branch -> `master` branch  
  Just one person working on the project? No time for process, just need to get builds **now**?  
  Alright, *alright*. I hear ya. Use this method instead.

**Short and sweet version:** Don't commit directly to `master` branch.

### Feature Branches

![workflow example](https://i.imgur.com/pwEqlCs.png)

Setting up feature branches is easy.  
After you've got the initial bootstrap done, create a `develop` branch and commit it. 
Then create a feature branch (does not need to be named anything in specific, just keep it short and descriptive) and work from there.

As you work, make commits to the feature branch and push them to Github as you see fit. 
CircleCi will run any unit tests and help you spot any issues along the way.

Once you've completed your feature, send a Pull Request from Github to merge into the `develop` branch.  
The project admin (maybe you?) will review this PR and decide to merge it if they see fit.

Once the PR is merged into the `develop` branch, the project admin will usually allow CircleCi to build a
prerelease module that can be used in an integration test for any projects that require this module.

After all integration tests are completed, the project admin will merge `develop` into `master` and cut a new git tag.

### Develop -> Master

![workflow example](https://i.imgur.com/Aq6SasP.png)

Don't have time for the whole feature branch method? That's fine. You can shortcut it and migrate to feature branches later if required.  

After the initial bootstrap, simply create and work on your `develop` branch. Commit directly to the `develop` branch and 
merge (PR or manually) into `master` when you're ready to create a new production-ready version by tagging off `master`.
