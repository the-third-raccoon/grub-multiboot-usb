## Prerequisites

### Hardware
- A USB drive (either empty or ready to be formatted).
- A computer running Linux (this setup used **Ubuntu 24.04.3 LTS**).

### Software
|Tool|Purpose|
|-|-|
| `GParted` | Partition USB drive |
| `gdisk` | Verify partition GUID (optional) |
| `wimtools`/`wimlib-utils` | Convert `.wim` file |
| `dosfstools` | FAT32 support |
| `mtools` | FAT32 support |

To install the required tools on **Ubuntu**, run:

```bash
sudo apt update
sudo apt install gparted gdisk wimtools dosfstools mtools
```

### Windows and Linux ISO Files (Optional)

The following ISO files were used for this setup. You can substitute other versions or add/remove ISOs as required:
- `Win11_25H2_English_x64.iso`
- `ubuntu-24.04.3-desktop-amd64.iso`
- `linuxmint-22.3-cinnamon-64bit.iso`

---
<table width="100%">
<td align="left"><a href="../README.md">⟸ Home</a></td>
<td align="right"><a href="partition-setup.md">Partition Setup ⟹</a></td>
</table>
