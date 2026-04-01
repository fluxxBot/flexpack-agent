# Directory Tree Checksum Template

For PMs that store packages as directories (rare — most use archives):

```go
import (
    "crypto/sha256"
    "io"
    "path/filepath"
)

func (pm *PackManager) calculateDirectoryChecksum(dirPath string) (string, error) {
    hasher := sha256.New()
    err := filepath.WalkDir(dirPath, func(path string, d os.DirEntry, err error) error {
        if err != nil { return nil }
        relPath, _ := filepath.Rel(dirPath, path)
        hasher.Write([]byte(relPath))
        if !d.IsDir() {
            file, err := os.Open(path)
            if err != nil { return err }
            defer file.Close()
            _, err = io.Copy(hasher, file)
            return err
        }
        return nil
    })
    if err != nil { return "", err }
    return fmt.Sprintf("%x", hasher.Sum(nil)), nil
}
```

Note: Only SHA256 computed for directories. SHA1 and MD5 returned empty.
