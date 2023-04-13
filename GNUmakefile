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

# Find out which EPICS versions to build.
INSTALLED_EPICS_VERSIONS := $(sort $(patsubst ${EPICS_LOCATION}/base-%,%,$(realpath $(wildcard ${EPICS_LOCATION}/base-*[0-9]))))
EPICS_VERSIONS = $(filter-out ${EXCLUDE_VERSIONS:=%},${DEFAULT_EPICS_VERSIONS})
MISSING_EPICS_VERSIONS = $(filter-out ${BUILD_EPICS_VERSIONS},${EPICS_VERSIONS})
BUILD_EPICS_VERSIONS = $(filter ${INSTALLED_EPICS_VERSIONS},${EPICS_VERSIONS})
$(foreach v,$(sort $(basename $(basename $(basename ${BUILD_EPICS_VERSIONS}))) $(basename $(basename ${BUILD_EPICS_VERSIONS})) $(basename ${BUILD_EPICS_VERSIONS})),$(eval EPICS_VERSIONS_$v=$(filter $v.%,${BUILD_EPICS_VERSIONS})))

SUBMODULES:=$(foreach f,$(wildcard .gitmodules),$(shell awk '/^\[submodule/ { print gensub(/["\]]/,"","g",$$2) }' $f))

# Check only version of files needed to build the module. But which are they?
VERSIONCHECKFILES = $(filter-out /% -none-, $(USERMAKEFILE) $(wildcard *.db *.template *.subs *.dbd *.cmd *.iocsh) ${SOURCES} ${DBDS} ${TEMPLATES} ${SCRIPTS} $($(filter SOURCES_% DBDS_%,${.VARIABLES})))
VERSIONCHECKFILES += ${SUBMODULES}
VERSIONCHECKCMD = ${MAKEHOME}/getVersion.pl ${VERSIONDEBUGFLAG} ${VERSIONCHECKFILES}
LIBVERSION = $(or $(filter-out test,$(shell ${VERSIONCHECKCMD} 2>/dev/null)),${USER},test)
VERSIONDEBUGFLAG = $(if ${VERSIONDEBUG}, -d)
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

# Some shell commands:
RMDIR = rm -rf
LN = ln -s
EXISTS = test -e
NM = nm
RM = rm -f
MKDIR = umask 002; mkdir -p -m 775

clean::
	$(RMDIR) O.*

clean.%::
	$(RMDIR) $(wildcard O.*${@:clean.%=%}*)

distclean: clean


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
  
