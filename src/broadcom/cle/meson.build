# Copyright © 2017 Broadcom
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

# [version, cle XML version]
v3d_versions = [
  [21, 21],
  [33, 33],
  [41, 33],
  [42, 33]
]

v3d_xml_files = []
v3d_xml_pack = []
foreach _v : v3d_versions
  v = _v[0]
  xmlver = _v[1]
  f = 'v3d_packet_v@0@.xml'.format(xmlver)
  _name = 'v3d_packet_v@0@_pack.h'.format(v)
  if not v3d_xml_files.contains(f)
    v3d_xml_files += f
  endif
  v3d_xml_pack += custom_target(
    _name,
    input : ['gen_pack_header.py', f],
    output : _name,
    command : [prog_python, '@INPUT@', '@0@'.format(v)],
    capture : true,
  )
endforeach

v3d_xml_h = custom_target(
  'v3d_xml.h',
  input : ['../../intel/genxml/gen_zipped_file.py', v3d_xml_files],
  output : 'v3d_xml.h',
  command : [prog_python, '@INPUT@'],
  capture : true,
)

expat_args = []
if dep_expat.found()
  expat_args += '-DWITH_LIBEXPAT=1'
endif

libbroadcom_cle = static_library(
  ['broadcom_cle', v3d_xml_h],
  'v3d_decoder.c',
  include_directories : [inc_include, inc_src, inc_mapi, inc_mesa, inc_gallium, inc_gallium_aux, inc_broadcom],
  c_args : [no_override_init_args, expat_args],
  gnu_symbol_visibility : 'hidden',
#   gnu_symbol_visibility : 'default',
  dependencies : [dep_libdrm, dep_valgrind, dep_expat, dep_zlib],
  build_by_default : false,
)
