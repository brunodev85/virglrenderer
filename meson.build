#############################################################################
#
# Copyright (C) 2019 Collabora Ltd
#
# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the
# Software is furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included
# in all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
# OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR
# OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
# ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
# OTHER DEALINGS IN THE SOFTWARE.
#

project(
   'virglrenderer', 'c',
   version: '0.8.1',
   license : 'MIT',
   meson_version : '>= 0.46',
   default_options : ['buildtype=release', 'b_ndebug=if-release',
                      'warning_level=3', 'c_std=gnu11']
)

# To change only before doing a release:
#
# 1. Incrememnt the revision
# 2. If the interface was changed in an compatible way increment the
#    interface age
# 3. If the ABI has changed in an incompatible way increment the binary_age
#    and set revision and interface_age to zero

binary_age    = 1
interface_age = 4
revision      = 1

cc = meson.get_compiler('c')

add_project_arguments('-DHAVE_CONFIG_H=1', language : 'c')
add_project_arguments('-D_GNU_SOURCE=1', language : 'c')

warnings = [
   '-Werror=implicit-function-declaration',
   '-Werror=missing-prototypes',
   '-Wmissing-prototypes',
   '-Werror=int-to-pointer-cast'
]

foreach w : warnings
   if cc.has_argument(w)
      add_project_arguments(w, language : 'c')
   endif
endforeach

prog_python = import('python').find_installation('python3')

thread_dep = dependency('threads')
epoxy_dep = dependency('epoxy', version: '>= 1.5.4')
m_dep = cc.find_library('m')

conf_data = configuration_data()
conf_data.set('VERSION', '0.8.1')

if cc.has_header('sys/uio.h')
   conf_data.set('HAVE_SYS_UIO_H', 1)
endif

if cc.has_header('dlfcn.h')
   conf_data.set('HAVE_DLFCN_H', 1)
endif

if cc.has_header('pthread.h')
   conf_data.set('HAVE_PTHREAD', 1)
endif

if cc.has_header('sys/eventfd.h')
   conf_data.set('HAVE_EVENTFD_H', 1)
endif

debug_enabled = get_option('debug')

if debug_enabled
   add_global_arguments('-DDEBUG=1', language : 'c')
endif

if cc.compiles('void __attribute__((hidden)) func() {}')
   conf_data.set('HAVE_FUNC_ATTRIBUTE_VISIBILITY', 1)
endif

configure_file(input : 'config.h.meson',
               output : 'config.h',
               configuration : conf_data)

pkgconf_data = configuration_data()
pkgconf_data.set('PACKAGE_VERSION', meson.project_version())
pkgconf_data.set('prefix', get_option('prefix'))
pkgconf_data.set('exec_prefix', '${prefix}')
pkgconf_data.set('libdir', '${prefix}/' + get_option('libdir'))
pkgconf_data.set('includedir', '${prefix}/' + get_option('includedir'))

pkg_config = configure_file(input : 'virglrenderer.pc.in',
                            output : 'virglrenderer.pc',
                            configuration : pkgconf_data)

install_data(pkg_config,
             install_dir: get_option('libdir') + '/pkgconfig')

inc_configuration = include_directories('.')

subdir('src')

lines = [
   '',
   'prefix:    ' + get_option('prefix'),
   'libdir:    ' + get_option('libdir'),
   '',
   'c_args:    ' + (' ').join(get_option('c_args')),
   '',
]

lines += 'debug:     ' + (debug_enabled ? 'yes' : 'no' )

indent = '   '
summary = indent + ('\n' + indent).join(lines)
message('\n\nConfiguration summary:\n@0@\n'.format(summary))
