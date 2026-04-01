# Single File Checksum Template

```go
import "github.com/jfrog/jfrog-client-go/utils/io/crypto"

func (pm *PackManager) calculateFileChecksum(filePath string) (sha1, sha256, md5 string, err error) {
    fileDetails, err := crypto.GetFileDetails(filePath, true)
    if err != nil {
        return "", "", "", fmt.Errorf("checksum calculation failed for %s: %w", filePath, err)
    }
    log.Debug("Calculated checksum for:", filePath)
    return fileDetails.Checksum.Sha1, fileDetails.Checksum.Sha256, fileDetails.Checksum.Md5, nil
}
```
