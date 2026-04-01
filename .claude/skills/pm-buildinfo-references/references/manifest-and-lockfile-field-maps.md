# Manifest and Lock File Field Maps

Go struct definitions for parsing each PM's manifest and lock files.

## Cargo

```go
// Cargo.toml
type CargoManifest struct {
    Package         CargoPackage                `toml:"package"`
    Dependencies    map[string]interface{}      `toml:"dependencies"`
    DevDependencies map[string]interface{}      `toml:"dev-dependencies"`
    BuildDeps       map[string]interface{}      `toml:"build-dependencies"`
}
type CargoPackage struct {
    Name    string `toml:"name"`
    Version string `toml:"version"`
}

// Cargo.lock
type CargoLock struct {
    Package []CargoLockPackage `toml:"package"`
}
type CargoLockPackage struct {
    Name         string   `toml:"name"`
    Version      string   `toml:"version"`
    Source       string   `toml:"source"`
    Checksum     string   `toml:"checksum"`
    Dependencies []string `toml:"dependencies"`
}
```

## Poetry

```go
// pyproject.toml
type PyProject struct {
    Tool struct {
        Poetry struct {
            Name         string                 `toml:"name"`
            Version      string                 `toml:"version"`
            Description  string                 `toml:"description"`
            Authors      []string               `toml:"authors"`
            Dependencies map[string]interface{} `toml:"dependencies"`
        } `toml:"poetry"`
    } `toml:"tool"`
}
// Note: dev dependencies in [tool.poetry.group.dev.dependencies]

// poetry.lock
type PoetryLock struct {
    Package []PoetryLockPackage `toml:"package"`
}
type PoetryLockPackage struct {
    Name            string                 `toml:"name"`
    Version         string                 `toml:"version"`
    Description     string                 `toml:"description"`
    Category        string                 `toml:"category"`
    Optional        bool                   `toml:"optional"`
    PythonVersions  string                 `toml:"python-versions"`
    Dependencies    map[string]interface{} `toml:"dependencies"`
}
```

## Helm

```go
// Chart.yaml
type HelmChart struct {
    APIVersion   string           `yaml:"apiVersion"`
    Name         string           `yaml:"name"`
    Version      string           `yaml:"version"`
    Dependencies []HelmDependency `yaml:"dependencies"`
}
type HelmDependency struct {
    Name       string `yaml:"name"`
    Version    string `yaml:"version"`
    Repository string `yaml:"repository"`
    Condition  string `yaml:"condition"`
    Alias      string `yaml:"alias"`
}

// Chart.lock
type HelmChartLock struct {
    Dependencies []HelmLockDep `yaml:"dependencies"`
    Digest       string        `yaml:"digest"`
    Generated    string        `yaml:"generated"`
}
type HelmLockDep struct {
    Name       string `yaml:"name"`
    Version    string `yaml:"version"`
    Repository string `yaml:"repository"`
}
```

## Maven

```go
// pom.xml
type MavenPOM struct {
    XMLName              xml.Name             `xml:"project"`
    GroupId              string               `xml:"groupId"`
    ArtifactId           string               `xml:"artifactId"`
    Version              string               `xml:"version"`
    Packaging            string               `xml:"packaging"`
    Modules              MavenModules         `xml:"modules"`
    Dependencies         MavenDependencies    `xml:"dependencies"`
    DependencyManagement MavenDepMgmt         `xml:"dependencyManagement"`
    DistributionMgmt     MavenDistMgmt        `xml:"distributionManagement"`
    Parent               *MavenParent         `xml:"parent"`
}
type MavenModules struct {
    Module []string `xml:"module"`
}
type MavenDependencies struct {
    Dependency []MavenDep `xml:"dependency"`
}
type MavenDep struct {
    GroupId    string `xml:"groupId"`
    ArtifactId string `xml:"artifactId"`
    Version    string `xml:"version"`
    Scope      string `xml:"scope"`
    Type       string `xml:"type"`
    Classifier string `xml:"classifier"`
    Optional   string `xml:"optional"`
}
type MavenDistMgmt struct {
    Repository         MavenRepo `xml:"repository"`
    SnapshotRepository MavenRepo `xml:"snapshotRepository"`
}
type MavenRepo struct {
    Id  string `xml:"id"`
    URL string `xml:"url"`
}
type MavenParent struct {
    GroupId    string `xml:"groupId"`
    ArtifactId string `xml:"artifactId"`
    Version    string `xml:"version"`
}
type MavenDepMgmt struct {
    Dependencies MavenDependencies `xml:"dependencies"`
}
```

## Conan

```go
// conan.lock (JSON)
type ConanLock struct {
    Version    string                   `json:"version"`
    Requires   []string                 `json:"requires"`
    BuildRequires []string              `json:"build_requires"`
    PythonRequires []string             `json:"python_requires"`
}

// conan graph info --format=json output
type ConanGraphInfo struct {
    Graph struct {
        Nodes map[string]ConanNode `json:"nodes"`
    } `json:"graph"`
}
type ConanNode struct {
    Ref         string            `json:"ref"`     // "name/version"
    PackageId   string            `json:"package_id"`
    Context     string            `json:"context"` // "host", "build", "test"
    Dependencies map[string]struct {
        Ref string `json:"ref"`
    } `json:"dependencies"`
}
```

## pnpm

```go
// pnpm ls -r --depth Infinity --json output
type PnpmLsOutput []PnpmProject

type PnpmProject struct {
    Name         string                       `json:"name"`
    Version      string                       `json:"version"`
    Path         string                       `json:"path"`
    Private      bool                         `json:"private"`
    Dependencies map[string]PnpmLsDependency  `json:"dependencies"`
    DevDependencies map[string]PnpmLsDependency `json:"devDependencies"`
}

type PnpmLsDependency struct {
    From         string                       `json:"from"`
    Version      string                       `json:"version"`
    Resolved     string                       `json:"resolved"`    // tarball URL
    Dependencies map[string]PnpmLsDependency  `json:"dependencies"`
}

// package.json
type PackageJSON struct {
    Name            string            `json:"name"`
    Version         string            `json:"version"`
    Dependencies    map[string]string `json:"dependencies"`
    DevDependencies map[string]string `json:"devDependencies"`
    PublishConfig   *struct {
        Registry string `json:"registry"`
    } `json:"publishConfig"`
}
```

## Gradle

Gradle has no structured lock file. Dependencies are parsed via:
1. **CLI**: `gradle :module:dependencies --configuration runtimeClasspath` → text tree output
2. **Regex from build.gradle**: Three notation styles:
   - String: `implementation("group:artifact:version")` or `implementation 'group:artifact:version'`
   - Map: `implementation(group: 'x', name: 'y', version: 'z')`
   - Project: `implementation(project(':module'))`

## Go

```go
// go.mod (line-based parsing)
// require (
//     github.com/foo/bar v1.2.3
//     github.com/baz/qux v0.1.0
// )

// go.sum (line-based: module version hash)
// github.com/foo/bar v1.2.3 h1:abc123=
// github.com/foo/bar v1.2.3/go.mod h1:def456=
```
