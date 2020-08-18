# liquibase-includeAll-changeLogDirectory

This project demonstrates a bug when you configure the liquibase-maven-plugin with a
changeLogDirectory and attempt to use the includeAll element.

The `errorIfMissingOrEmpty` attribute is true to demonstrate that the plugin is able to find files in the includeALl
path but cannot process the files because [FileSystemResourceAccessor](https://github.com/liquibase/liquibase/blob/3.10.x/liquibase-core/src/main/java/liquibase/resource/FileSystemResourceAccessor.java#L167)
leaves a leading slash when converting a path that has a changeLogDirectory specified.

# Usage

The only pre-requisite is Java 8 or later. Clone this repo then run `mvnw liquibase:status` and
you'll see the error "liquibase.exception.SetupException: File does not exist: file:/includeAll/sample.sql" even
though the plugin found sample.sql.

# Pull request

A pull request has been submitted that adds one to the length of the changeLogDirectory canonical path, so the substring
removes the leading slash when converting a path that has a changeLogDirectory specified.
