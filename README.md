# vxlsdk-rel

ASVXL depth camera SDK — prebuilt binaries and headers (release distribution of [vxlsdk](https://git.asfly.ltd/VXTeam/asvxl)).

## Usage

### With VxlROS2

```bash
cd /path/to/VxlROS2/
git clone https://github.com/asvoxel/vxlsdk-rel.git vxlsdk
colcon build
```

### Standalone

```bash
export VXL_SDK_DIR=/path/to/vxlsdk-rel
gcc -o demo demo.c -I${VXL_SDK_DIR}/include -L${VXL_SDK_DIR}/lib/linux -lasvxl -lusb-1.0 -lpthread
```

## Structure

```
vxlsdk-rel/
├── include/          # C/C++ API headers
├── lib/
│   ├── linux/        # Linux static library
│   └── darwin/       # macOS static library
├── examples/         # Example programs
├── docs/             # Documentation
├── VERSION           # Build metadata
└── LICENSE
```

## Links

- VxlROS2: https://github.com/asvoxel/vxlros2
- Website: https://asvoxel.com
