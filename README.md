# ninja-framework-with-gradle

This project has an example of integration of ninja framework with Gradle. 

Here we will find an example of how to integrate Ninja Framework with Gretty plugin that offers the same functionalities that ninja-maven-plugin offers (Most known because of his superdev mode)

- [Ninja Framework](http://www.ninjaframework.org/)
- [Ninja Maven Plugin](http://www.ninjaframework.org/documentation/basic_concepts/super_dev_mode.html)
- [Gretty webpage](https://github.com/akhikhl/gretty)


The easiest way and the one that will require less work to maintain (when adding new modules) is decribed in the main build.gradle file:

```Gradle
apply plugin: 'org.akhikhl.gretty'
gretty {
	/** Sets the context path */
	contextPath = '/'

	/** Sets another project (in the same project tree) as overlay source. */
 	overlay ':ninja-webapp'

	/** Adds all the subprojects' source code dirs of this project to be scaned when the code
	 *  changes. In this way, you don't need ot specify a scanDir per project
	 */
	(project.getSubprojects() - project(':ninja-webapp') ).sourceSets.main.java.srcDirs.each { sourceDirs ->
		sourceDirs.each{
			scanDir "${it}"
		}
	}
}
```

The gretty plugin should also be applied to the overlay webapp (in this scenario, ninja-webapp) as we can check in the ninja-webapp's build.gradle file


## In order to test this solutin, you will just need to execute the following command:

```Batchfile
 ./gradlew jettyRun
```

Then, go to the submodule and do a change in the Test class. You'll then see in the terminal that the new source will be loaded.
 
To get more information of how to use Gretty, please refer to the [Gretty documentation](http://akhikhl.github.io/gretty-doc/)
