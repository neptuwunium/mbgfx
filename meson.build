project(
    'bgfx',
    ['c', 'cpp'],
    default_options: [
        'cpp_std=c++20',
        'unity=on',
        'optimization=2',
        'warning_level=0',
    ],
)

fs = import('fs')
sources = []
foreach s : fs.read('sources.txt').strip().split('\n')
    sources += files('src/' + s + '.cpp')
endforeach


bgfx_deps = [
    dependency('bimg'),
    dependency('bx'),
    dependency('h264'),
    dependency('directx-headers'),
    dependency('metalhpp'),
    dependency('renderdoc'),
    dependency('khronos'),
    dependency('dawn'),
]

if host_machine.system() == 'darwin'
    bgfx_deps += [
        dependency('OpenGL', method: 'extraframework'),
        dependency('Cocoa', method: 'extraframework'),
        dependency('QuartzCore', method: 'extraframework'),
        dependency('IOKit', method: 'extraframework'),
        dependency('Metal', method: 'extraframework'),
        dependency('CoreVideo', method: 'extraframework'),
        dependency('CoreMedia', method: 'extraframework'),
        dependency('VideoToolbox', method: 'extraframework'),
    ]
else
    bgfx_deps += dependency('gl')
endif
bgfx_args = []

if host_machine.system() == 'linux' and get_option('with_wayland')
    bgfx_args += '-DWL_EGL_PLATFORM=1'
endif

if host_machine.system() == 'darwin'
    bgfx_args += '-xobjective-c++'
endif

bgfx_lib = static_library(
    'bgfx',
    sources,
    pic: host_machine.system() == 'linux',
    include_directories: 'include',
    dependencies: bgfx_deps,
    cpp_args: bgfx_args,
)

bgfx_dep = declare_dependency(
    link_with: bgfx_lib,
    include_directories: 'include',
)

meson.override_dependency('bgfx', bgfx_dep)

if get_option('build_tools')
    subdir('tools')
endif
