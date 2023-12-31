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

subdir('gallium')

virglrenderer_sources = [
   'iov.c',
   'virgl_hw.h',
   'virgl_protocol.h',
   'virglrenderer.c',
   'virglrenderer.h',
   'vrend_blitter.c',
   'vrend_blitter.h',
   'vrend_decode.c',
   'vrend_formats.c',
   'vrend_iov.h',
   'vrend_object.c',
   'vrend_object.h',
   'vrend_renderer.c',
   'vrend_renderer.h',
   'vrend_shader.c',
   'vrend_shader.h',
   'vrend_strbuf.h',
   'vrend_tweaks.c',
   'vrend_tweaks.h',
   'vrend_util.h',
]

virglrenderer_depends = [
   gallium_dep,
   epoxy_dep,
   thread_dep,
   m_dep
]

virgl_server_sources = [
   '../server/virgl_server_shm.c',
   '../server/virgl_server_shm.h',
   '../server/virgl_server.c',
   '../server/virgl_server_renderer.c',
   '../server/virgl_server_protocol.h',
   '../server/virgl_server.h'
]

virglrenderer_sources += virgl_server_sources

libvirglrenderer = library(
   'virglrenderer',
   virglrenderer_sources,
   include_directories: [inc_gallium, inc_configuration],
   dependencies : virglrenderer_depends,
   install : true
)

libvirglrenderer_inc = [
   inc_gallium,
   inc_configuration,
   include_directories('.')
]

libvirglrenderer_dep = declare_dependency(
   link_with: libvirglrenderer,
   include_directories: libvirglrenderer_inc
)

install_headers('virglrenderer.h', subdir : 'virgl')
