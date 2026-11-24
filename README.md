# WizeStream Plugins

The plugin catalog for [WizeStream](https://github.com/wizdom13/WizeStream), with
community-contributed entries for additional streaming services. The catalog is
published as [`index.json`](index.json); plugin packages are hosted by their authors.

> Plugins execute inside WizeStream with the app's permissions. Catalog inclusion
> is not a security audit. Install packages only from publishers you trust.

## Install a plugin

Use a WizeStream build that includes **Settings → Plugins**. External plugins
require **Android 7 / API 24 or newer**; individual packages may require a newer
Android or host version. The app's built-in services remain available on Android 6.

1. Open **Settings → Plugins** and enable **Allow plugins** after reading the
   explanation.
2. In builds containing the WizeStream catalog change, **WizeStream Plugins** is
   included automatically in **Repositories**. No additional repository is needed.
   On an earlier plugin-enabled build, use **Add repository** with the raw JSON URL:

   ```text
   https://raw.githubusercontent.com/wizdom13/WizeStream-Plugins/main/index.json
   ```

3. Use **Refresh repositories** if needed, select a compatible plugin under
   **Available**, and confirm its installation.
4. Restart WizeStream when prompted to load the new service.

Use the raw URL above, not GitHub's `blob/main/index.json` page, which returns HTML.
A Windows checkout path is useful for editing this catalog but cannot be used as
an Android repository URL.

Installed plugins can be updated, configured or uninstalled from **Installed**.
Package changes take effect after restarting the app. WizeStream verifies each
package's hash and signature and pins its publisher key on first installation;
updates must retain that key and increase `versionCode`.

Services already bundled with WizeStream do not need plugins. External packages
cannot replace a built-in provider. For compatibility and profile behavior, see
the [WizeStream plugin guide](https://github.com/wizdom13/WizeStream/blob/pipe/docs/plugins.md).

## Available services

| Plugin | Description | Author | Source |
| --- | --- | --- | --- |
| Twitch | Access Twitch through an external service plugin. | Jonas Mohr | [TwitchPipePipePlugin](https://github.com/MohrJonas/TwitchPipePipePlugin) |

[`index.json`](index.json) is the authoritative list of package versions, download
URLs and signatures. The app checks package compatibility before installation.

## Contribute or update a plugin

1. Implement, build and sign the plugin. For a native WizeStream plugin, start with
   the [WizeStream Plugin Template](https://github.com/wizdom13/WizeStream-Plugin-Template).
2. Test the packaged plugin with a compatible WizeStream build, including its
   minimum supported Android version.
3. Publish the signed `.ppp` file at a stable HTTPS URL, such as a versioned GitHub
   release asset. Keep its source code publicly available; closed-source plugins
   are not accepted.
4. Open a pull request updating `index.json` and, for a new service, the table above.
   Include the source repository, release notes and the host/Android versions tested.

Copy package identity, versions, compatibility requirements, hash, signature and
public key from the publisher's generated catalog. Metadata must match the signed
package's `plugin.json`. Use an absolute HTTPS package URL when copying an entry
from another catalog; a relative URL would resolve against this repository.

### Supported contracts

| Contract | `host` | `apiVersion` | Starting point |
| --- | --- | --- | --- |
| Native WizeStream | `"wizestream"` | `"2.0"` | [WizeStream Plugin Template](https://github.com/wizdom13/WizeStream-Plugin-Template) |
| PipePipe-compatible | `"pipepipe"`, or omitted | `"1.0"` or `"1.1"` | A package built for the supported PipePipe contract |

Native entries also carry `minHostVersionCode`, `minAndroidSdk` and any
`requiredCapabilities` from the signed manifest. API compatibility alone does not
guarantee that every external package works on every supported device. Keep the
publisher's declared requirements intact and test the actual package.

### Catalog structure

The root object uses `formatVersion: 1`, a catalog `name`, and a `plugins` array.
This example describes a PipePipe-compatible entry; its cryptographic values are
placeholders and must be replaced with those generated for the published package.

```json
{
  "formatVersion": 1,
  "name": "WizeStream Plugins",
  "plugins": [
    {
      "pluginId": "org.example.myservice",
      "name": "MyService",
      "versionCode": 3,
      "versionName": "1.2.0",
      "host": "pipepipe",
      "apiVersion": "1.0",
      "author": "Jane Doe",
      "package": "https://example.com/myservice-3.ppp",
      "sha256": "<64 hexadecimal characters>",
      "signature": "<Base64 Ed25519 signature over the raw SHA-256 digest>",
      "publicKey": "<Base64 raw 32-byte Ed25519 public key>",
      "description": "Short description",
      "nsfw": false
    }
  ]
}
```

`description` and `nsfw` are optional. `sha256` covers the exact published archive;
the signature is over the raw **32 digest bytes**, not their hexadecimal text.
The public key in the catalog is the raw key, not a PEM or X.509 document. The
native template generates these values in the host's required format.

### Contribution rules

- Keep `pluginId` stable and increase `versionCode` for each update.
- Retain the publisher's signing key; a different key is rejected for an already
  trusted plugin identity, including after uninstalling it.
- The plugin must perform its advertised function and disclose any data collection.
- No malware, ad injection or tracking.
- Entries may be removed if a plugin is abandoned, broken or harmful.

Report service-specific problems to the plugin author. Report a broken catalog
entry through this repository's [issues](https://github.com/wizdom13/WizeStream-Plugins/issues).

## License

See [LICENSE](LICENSE) for the GNU GPLv3 license included with this repository.
Each plugin retains its own license and attribution.
