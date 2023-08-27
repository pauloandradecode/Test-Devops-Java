Test-Devops-Java
===

## Pipelines

### Artifacs

Para publicar un artefacto mediante pipelines debemos indicar en el archivo "azure-pipelines.yml" que el paquete sera compilado y publicado en artifacs.-

```
goals: 'package  deploy:deploy-file'
```

Damos permisos para loguearnos en el artefacto que creamos en DevOps.-

```
mavenAuthenticateFeed: true
```

Para optimizar el proceso de compilado y publicación del artefacto, utilizamos los siguientes parametros.-

```
options: '-X -P azure_artifacts -Dpackaging="jar" -DrepositoryId="Common_Artifac" -Durl="https://pkgs.dev.azure.com/paulo866/56086bd2-562a-4422-a346-2e7d375f05af/_packaging/Common_Artifac/maven/v1" -DgroupId="com.digitalthinking" -DartifactId="common-library" -Dversion="0.0.2-SNAPSHOT" -Dfile="$(System.DefaultWorkingDirectory)/target/common-library-0.0.2-SNAPSHOT.jar"'
```

La explicación es la siguiente.-

* -X.- se utiliza para hacer un DEBUG y en caso de error, se mostrara con más detalles el mismo.
* -P azure_artifacts.- indicamos el perfil a utilizar (el perfil se establece en el archivo pom.xml), en este caso el perfil utilizado es azure_artifacts.
* -Dpackaging="jar".- (Opciones de publicación) Tipo de paquete para el deploy.
* -DrepositoryId="Common_Artifac".- Nombre del repositorio (feed) donde se publicara, se configura en Azure DevOps.
* -Durl="https://pkgs.dev.azure.com/paulo866/56086bd2-562a-4422-a346-2e7d375f05af/_packaging/Common_Artifac/maven/v1".- Url del Feed.
* -DgroupId="com.digitalthinking".- (Parametro del archivo pom)
* -DartifactId="common-library"
* -Dversion="0.0.2-SNAPSHOT"
* -Dfile="$(System.DefaultWorkingDirectory)/target/common-library-0.0.2-SNAPSHOT.jar"'

En la configuración del archivo "pom.xml", agregamos la siguiente configuración antes de build.-

```
<profiles>
    <profile>
        <id>
        azure_artifacts
        </id>
        <repositories>
            <repository>
                <id>Common_Artifac</id>
                <url>https://pkgs.dev.azure.com/paulo866/56086bd2-562a-4422-a346-2e7d375f05af/_packaging/Common_Artifac/maven/v1</url>
                <releases>
                    <enabled>true</enabled>
                </releases>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
            </repository>
        </repositories>
        <build>
        ...
        </build>
    </profile>
</profiles>
...
```
