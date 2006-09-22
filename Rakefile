#-*-ruby-*-
#
# Copyright (C) 2006 Peter McLain <peter.mclain@gmail.com>
#
# This program is distributed under the terms of the MIT license.
# See the included MIT-LICENSE file for the terms of this license.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
# OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
# IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
# CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
# TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
# SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

require 'rake/clean'
require 'rbconfig'

$INCLUDES = "-I#{Config::CONFIG['archdir']}"
$CFLAGS   = "#{Config::CONFIG['CFLAGS']}"
$LDFLAGS  = "-l#{Config::CONFIG["RUBY_SO_NAME"]}"
$LDCMD    = "#{Config::CONFIG['LDSHARED']}"

case RUBY_PLATFORM
when /darwin/
    $INCLUDES << ' -F/System/Library/Frameworks'
    $LDFLAGS  << ' -framework GLUT -framework OpenGL'
    $LDCMD = 'cc -bundle'
else
    $INCLUDES << ' -L/usr/lib'
    $LDFLAGS  << ' -lglut -lGLU -lGL' # must not be for all targets
end

GL_LIB   = "ext/gl/gl.#{Config::CONFIG['DLEXT']}"
GLU_LIB  = "ext/glu/glu.#{Config::CONFIG['DLEXT']}"
GLUT_LIB = "ext/glut/glut.#{Config::CONFIG['DLEXT']}"
LIBS     = [ GL_LIB, GLU_LIB, GLUT_LIB ]

CLEAN.include( 'ext/**/*.o' )
CLOBBER.include( LIBS )

desc "Create #{LIBS}"
task :default => LIBS

rule '.o' => '.c' do |t|
    cmd = "cc #{$INCLUDES} #{$CFLAGS} -c -o #{t.name} #{t.source}"
    puts "============== #{cmd}"
    sh cmd
end

desc "Create the OpenGL library (#{GL_LIB})"
file GL_LIB => [ 'ext/gl/gl.o', 'ext/common/rbogl.o' ] do |t|
    cmd = "#{$LDCMD} #{$LDFLAGS} #{$CFLAGS} -o #{t.name} #{t.prerequisites.join(' ')}"
    puts "============== #{cmd}"
    sh cmd
end


# TODO: This is a cut-n-paste of the GL_LIB target.  Need to define a rule
# to build them
desc "Create the GLU library (#{GLU_LIB})"
file GLU_LIB => [ 'ext/glu/glu.o', 'ext/common/rbogl.o' ] do |t|
    cmd = "#{$LDCMD} #{$LDFLAGS} #{$CFLAGS} -o #{t.name} #{t.prerequisites.join(' ')}"
    puts "============== #{cmd}"
    sh cmd
end

    
# TODO: This is a cut-n-paste of the GL_LIB target.  Need to define a rule
# to build them
desc "Create the GLUT library (#{GLUT_LIB})"
file GLUT_LIB => [ 'ext/glut/glut.o' ] do |t|
    cmd = "#{$LDCMD} #{$LDFLAGS} #{$CFLAGS} -o #{t.name} #{t.prerequisites.join(' ')}"
    puts "============== #{cmd}"
    sh cmd
end

file 'ext/common/rbogl.o' => [ 'ext/common/rbogl.h', 'ext/common/rbogl.c' ]