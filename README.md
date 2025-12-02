# bootchain 
```mermaid
flowchart TD
    subgraph Silicon [Immutable ROM inside SoC (cannot be bricked)]
        A[Power-On Reset] --> B[Primary Bootloader\n(PBL / Boot ROM)]
    end

    subgraph eMMC / UFS Flash [User-flashable partitions]
        B --> C[Secondary Bootloader\n(XBL / Little Kernel / ABOOT / sbl)\n← This is what "fastboot flashing unlock" affects]
        C -->|normal boot| D[boot.img → Android System\n(or HyperOS / One UI / OxygenOS etc.)]
        C -->|Vol+ + Power\nor "adb reboot recovery"| E[recovery.img → Custom Recovery\n(TWRP / Lineage Recovery / OrangeFox)]
        C -->|Vol- + Power\nor "adb reboot bootloader"| F[Fastboot / Download Mode Screen]
    end

    subgraph Rescue Modes [SoC-level, survive bootloader death]
        B -->|EDL test points / deep flash cable| G[Qualcomm EDL (9008) Mode]
        B -->|Vol Down + USB (MediaTek)| H[MediaTek BROM Mode]
        B -->|Vol- + Vol+ + Power (Samsung)| I[Samsung Download Mode (Odin)]
    end

    style Silicon fill:#fff2e6,stroke:#b84700,stroke-width:2px
    style eMMC fill:#e6f3ff,stroke:#0066cc,stroke-width:2px
    style Rescue Modes fill:#ffeeee,stroke:#cc0000,stroke-width:2px
    style C fill:#ffcccc,stroke:#990000,stroke-width:3px
    style G fill:#ff9999,stroke:#990000
    style H fill:#ff9999,stroke:#990000
    style I fill:#ff9999,stroke:#990000

    click C "Bootloader partition – if erased or corrupted → fastboot disappears → recovery also unreachable" _self
```
