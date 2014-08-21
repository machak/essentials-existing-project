Using Hippo Essentials within an existing project
===========================
```
NOTE: before using essentials make sure:
* make sure you have no preview mount
* enable autoexport
* you have standard hippo setup (/site, /cms, /bootstrap directories)
* read limitations (below)
```

# Getting Started

### Clone project *
clone project into YOUR_PROJECT_DIR:
```
$ cd YOUR_PROJECT_DIR
$ git clone git@github.com:machak/essentials-existing-project.git essentials

```
### change following settings within: YOUR_PROJECT_DIR/essentials/src/main/resources/project-settings.xml file:
```
projectNamespace: namespace prefix used within your project, by default name can be located within : YOUR_PROJECT_DIR/bootstrap/configuration/src/main/resources/namespaces/YOUR_PROJECT_NAME.cnd
selectedBeansPackage: package name where HST beans are located e.g. org.example.beans
selectedComponentsPackage: package name where HST components are located e.g. org.example.components
selectedRestPackage: package name where HST rest classes are located e.g. org.example.rest
```

### add property to main pom (YOUR_PROJECT_DIR/pom.xml).
```
 <essentials.version>ESSENTIALS_VERSION</essentials.version>
```
### add managed dependencies to main pom:
```
     <dependency>
        <groupId>org.onehippo.cms7.essentials</groupId>
        <artifactId>hippo-essentials-plugin-api-implementation</artifactId>
        <version>${essentials.version}</version>
      </dependency>
      <dependency>
        <groupId>org.onehippo.cms7.essentials</groupId>
        <artifactId>hippo-essentials-plugin-api</artifactId>
        <version>${essentials.version}</version>
      </dependency>
      <dependency>
        <groupId>org.onehippo.cms7.essentials</groupId>
        <artifactId>hippo-components-cms-plugin-library</artifactId>
        <version>${essentials.version}</version>
      </dependency>
      <dependency>
        <groupId>org.onehippo.cms7.essentials</groupId>
        <artifactId>hippo-components-plugin-library</artifactId>
        <version>${essentials.version}</version>
      </dependency>
```
### add module to main pom:
```
  <module>essentials</module>
```
### add deployable to main pom:
```
<deployable>
  <location>${project.basedir}/essentials/target/essentials.war</location>
  <type>war</type>
  <properties>
    <context>/essentials</context>
  </properties>
</deployable>
```


### change parent pom settings and version of YOUR_PROJECT_DIR/essentials/pom.xml:
- version, group and artifact name


### add following dependencies to CMS pom (YOUR_PROJECT_DIR/cms/pom.xml):
```
    <dependency>
      <groupId>org.onehippo.cms7.essentials</groupId>
      <artifactId>hippo-components-cms-plugin-library</artifactId>
    </dependency>
    <dependency>
      <groupId>org.onehippo.cms7.essentials</groupId>
      <artifactId>hippo-essentials-plugin-api</artifactId>
    </dependency>
```
### add following dependencies to SITE project (YOUR_PROJECT_DIR/site/pom.xml) :
```
    <dependency>
      <groupId>org.onehippo.cms7.essentials</groupId>
      <artifactId>hippo-essentials-plugin-api</artifactId>
    </dependency>
    <dependency>
      <groupId>org.onehippo.cms7.essentials</groupId>
      <artifactId>hippo-components-plugin-library</artifactId>
    </dependency>
```

### build project && run project:
```
mvn clean package && mvn -P cargo.run
```


### Launch Essentials webapp

[http://localhost:8080/essentials](http://localhost:8080/essentials)





### Current limitations
# What will work:
- component installation (can be dragged droped from component catalog)
- templates installation
- tools (Image Gallery tool, Beanwriter, REST setup)

# What might work (see "Name conflicts"):
- sitemaps, pages and component configuration.

#Name/resource conflicts:

Name conflicts may happen during the installation process. If some resource already exists (e.g. in case of exiting File, JCR Node or registered Document type),
Hippo Essentials will not overwrite existing resource.
So, although your existing work will not be overwritten, this "partial" installation state could "cripple" your project. Think of things like:
- adding markup files (JSP or FreeMarker) which are never used
- additional HST component containers/workspaces
- adding of sample content

Because Hippo Essentials itself doesn't support roll-back feature, developers themselves should review and revert any unwanted changes.

### Terms used:
```
ESSENTIALS_VERSION= latest essentials version, check @Please check latest version on: http://www.onehippo.org/trails/essentials-trail/hippo-essentials-getting-started.html
YOUR_PROJECT_DIR = root of your project e.g. /home/users/joedoe/myhippoproject
YOUR_PROJECT_NAME= name of your project e.g. myhippoproject
```
