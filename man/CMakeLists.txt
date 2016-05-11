set(manpages_srcs
  8/ceph-osd.rst
  8/ceph-mds.rst
  8/ceph-mon.rst
  8/ceph-syn.rst
  8/crushtool.rst
  8/osdmaptool.rst
  8/monmaptool.rst
  8/ceph-conf.rst
  8/ceph-run.rst
  8/ceph.rst
  8/mount.ceph.rst
  8/ceph-create-keys.rst
  8/radosgw.rst
  8/radosgw-admin.rst
  8/ceph-authtool.rst
  8/rados.rst
  8/librados-config.rst
  8/ceph-clsinfo.rst
  8/ceph-debugpack.rst
  8/cephfs.rst
  8/ceph-dencoder.rst
  8/ceph-rest-api.rst
  8/ceph-post-file.rst)

if(HAVE_LIBFUSE)
  list(APPEND manpage_srcs
    8/ceph-fuse.rst
    8/rbd-fuse.rst)
endif()

if(WITH_RBD)
  list(APPEND manpage_srcs
	8/ceph-rbdnamer.rst
	8/rbd-nbd.rst
	8/rbd-replay-prep.rst
	8/rbd-replay.rst
	8/rbdmap.rst
	8/rbd.rst)
endif()

foreach(manpage ${manpage_srcs})
  string(REGEX MATCH "^[0-9]/" section ${manpage})
  get_filename_component(cmd ${manpage} NAME_WE)
  list(APPEND sphinx_input ${CMAKE_SOURCE_DIR}/doc/man/${manpage})
  list(APPEND sphinx_output ${CMAKE_BINARY_DIR}/doc/man/${cmd}.${section})
endforeach()

install(FILES ${sphinx_output} ceph_selinux.8
  DESTINATION ${CEPH_MAN_DIR}/man8)

add_custom_command(
  OUTPUT ${sphinx_output}
  COMMAND ${SPHINX_BUILD} -b man -d doctrees -c ${CMAKE_SOURCE_DIR}/man ${CMAKE_SOURCE_DIR}/doc/man ${CMAKE_BINARY_DIR}/doc/man
  DEPENDS ${sphinx_input})

add_custom_target(
  manpages ALL
  DEPENDS ${sphinx_output}
  COMMENT "docs building")
