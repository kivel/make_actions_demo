# Some configuration:
DEFAULT_EPICS_VERSIONS = 3.13.10 3.14.12 7.0.5
BUILDCLASSES = vxWorks
EPICS_MODULES ?= /ioc/modules
MODULE_LOCATION = ${EPICS_MODULES}/$(or ${PRJ},$(error PRJ not defined))/$(or ${LIBVERSION},$(error LIBVERSION not defined))
EPICS_LOCATION = /usr/local/epics

# Some shell commands:
LN = ln -s
EXISTS = test -e
NM = nm
RMDIR = rm -rf
RM = rm -f
CP = cp

# Some generated file names:
VERSIONFILE = ${PRJ}_version_${LIBVERSION}.c
REGISTRYFILE = ${PRJ}_registerRecordDeviceDriver.cpp
EXPORTFILE = ${PRJ}_exportAddress.c
SUBFUNCFILE = ${PRJ}_subRecordFunctions.dbd
DEPFILE = ${PRJ}.dep

# Clear potentially problematic environment variables.
BASH_ENV=
ENV=

# Function that removes duplicates without re-ordering (unlike sort):
define uniq
  $(eval seen :=) \
  $(foreach _,$1,$(if $(filter $_,${seen}),,$(eval seen += $_))) \
  ${seen}
endef
# Function that removes directories from list
define filter_out_dir
  $(shell perl -e 'foreach my $$x (qw($1)) {unless(-d "$$x") {print "$$x "}}')
endef
# Function that retains the certian portion of the header file path
# and then used as the subdirectory inside INSTALL_INCLUDE
#   param 1 - list of header files, "src/os/Linux/jconfig.h src/pv.h src/os/default/jpeglib.h"
#   param 2 - pattern "os/${OS_CLASS}/|os/default/"
#   return - file path with pattern retained, "os/Linux/jconfig.h pv.h os/default/jpeglib.h"
define os_include_dir
  $(shell perl -e 'use File::Basename;foreach my $$x (qw($1)) {if ($$x =~ m[(^|/)os/]) {if ($$x =~ m[os/(default|$2)/(.*)]) {print "os/$$1/$$2 "}} else {print(basename("$$x")," ")}}')
endef

MODULE=
PROJECT=
PRJDIR:=$(subst -,_,$(subst .,_,$(notdir $(patsubst %Lib,%,$(patsubst %/snl,%,$(patsubst %/src,%,${PWD}))))))
PRJ = $(strip $(or ${MODULE},${PROJECT},${PRJDIR}))
export PRJ

OS_CLASS_LIST = $(BUILDCLASSES)
export OS_CLASS_LIST

export ARCH_FILTER
export EXCLUDE_ARCHS
export MAKE_FIRST
export SUBMODULES
export USE_LIBVERSION

debug::
	@echo "INSTALLED_EPICS_VERSIONS = ${INSTALLED_EPICS_VERSIONS}"
	@echo "BUILD_EPICS_VERSIONS = ${BUILD_EPICS_VERSIONS}"
	@echo "MISSING_EPICS_VERSIONS = ${MISSING_EPICS_VERSIONS}"
	@echo "EPICS_VERSIONS_3.13 = ${EPICS_VERSIONS_3.13}"
	@echo "EPICS_VERSIONS_3.14 = ${EPICS_VERSIONS_3.14}"
	@echo "EPICS_VERSIONS_3.15 = ${EPICS_VERSIONS_3.15}"
	@echo "EPICS_VERSIONS_3.16 = ${EPICS_VERSIONS_3.16}"
	@echo "EPICS_VERSIONS_3 = ${EPICS_VERSIONS_3}"
	@echo "EPICS_VERSIONS_7 = ${EPICS_VERSIONS_7}"
	@echo "BUILDCLASSES = ${BUILDCLASSES}"
	@echo "LIBVERSION = ${LIBVERSION}"
	@echo "VERSIONCHECKFILES = ${VERSIONCHECKFILES}"
	@echo "ARCH_FILTER = ${ARCH_FILTER}"
	@echo "PRJ = ${PRJ}"
  
