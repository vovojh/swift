if(SWIFT_SOURCEKIT_USE_INPROC_LIBRARY)
  set(EXTRA_COMPILE_FLAGS "-I" "${SOURCEKITD_SOURCE_DIR}/include/sourcekitd")
  set(SOURCEKITD_LINK_LIBS sourcekitdInProc)
  set(INSTALLED_COMP sourcekit-inproc)
else()
  set(EXTRA_COMPILE_FLAGS "-F" "${SWIFT_LIBRARY_OUTPUT_INTDIR}")
  set(SOURCEKITD_LINK_LIBS sourcekitd)
  set(INSTALLED_COMP sourcekit-xpc-service)
endif()

add_swift_library(swiftSwiftLang SHARED
  SwiftLang.swift
  SourceKitdClient.swift
  SourceKitdRequest.swift
  SourceKitdResponse.swift
  SourceKitdUID.swift
  UIDs.swift.gyb

  DEPENDS sourcekitd-test
  PRIVATE_LINK_LIBRARIES ${SOURCEKITD_LINK_LIBS}
  SWIFT_COMPILE_FLAGS ${EXTRA_COMPILE_FLAGS}
  INSTALL_IN_COMPONENT ${INSTALLED_COMP}
  TARGET_SDKS ${SOURCEKIT_DEFAULT_TARGET_SDK}
  IS_STDLIB)
