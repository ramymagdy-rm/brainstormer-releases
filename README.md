# Installing The Brainstormer (Beta) directly

The Brainstormer for Windows 11 comes two ways: from the **Microsoft
Store** (recommended: signed by Microsoft, updated by the Store), and as
the **beta**, downloaded from this repo's Releases. The beta is signed
with The Brainstormer's own certificate, which Windows does not know, so
your PC has to trust it once. This takes an administrator.

Both can be installed side by side. Only one runs at a time, and the app
will tell you if it finds the other one, so you can keep just one.

## 1. Check and trust the certificate (once per PC)

Download `TheBrainstormer.cer` from the release. Before you trust it,
check it is the real one. In PowerShell:

```powershell
Get-PfxCertificate .\TheBrainstormer.cer | Format-List Subject, Thumbprint, NotAfter
```

It must print subject `CN=The Brainstormer`, thumbprint
`1D4A9A145910992BA9DB685FDF1CCCE65C89E26F`, valid until 2031-10-04. If
anything differs, stop and do not trust it. (This thumbprint is
published beside the file it checks: it catches a damaged or swapped
download, not a compromised repo.)

Then, in PowerShell **run as administrator**:

```powershell
Import-Certificate -FilePath .\TheBrainstormer.cer -CertStoreLocation Cert:\LocalMachine\TrustedPeople
```

Or without PowerShell: double-click `TheBrainstormer.cer`, **Install
Certificate…**, **Local Machine**, **Place all certificates in the
following store**, **Browse…**, **Trusted People**, **Finish**.

Trusting it lets Windows install any package signed with this
certificate, and such a package runs as a normal desktop app with your
permissions, as the Brainstormer does. Only the Brainstormer's owner
holds the key.

## 2. Install the beta

Download `TheBrainstormer-Beta-<version>.msix` and double-click it, then
**Install**. Or in PowerShell (no administrator needed):

```powershell
Add-AppxPackage .\TheBrainstormer-Beta-<version>.msix
```

If the installer says the **Windows App Runtime** is missing, install
its 2.x runtime (x64) from Microsoft's
[Windows App SDK downloads](https://learn.microsoft.com/windows/apps/windows-app-sdk/downloads),
then install the beta again. The widget needs it.

A newer beta installs over the old one the same way, keeping what the
app stored on your PC (your sign-in).

## 3. After installing

The Brainstormer starts in the tray. Sign in through your browser, then
add its widget from the Widgets Board (Win+W, **+**). If the widget is not
in the list yet, sign out of Windows and back in once: the board reads
new widgets when it starts.

## Removing it

Uninstall **The Brainstormer (Beta)** from **Settings → Apps → Installed
apps**. To stop trusting the certificate too, open `certlm.msc` as
administrator, go to **Trusted People → Certificates** and delete
**The Brainstormer**.
