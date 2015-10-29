protocols = 									\
	unstable/pointer-gestures/pointer-gestures-unstable-v1.xml		\
	unstable/fullscreen-shell/fullscreen-shell-unstable-v1.xml		\
	unstable/linux-dmabuf/linux-dmabuf-unstable-v1.xml			\
	unstable/presentation-timing/presentation-timing-unstable-v1.xml	\
	unstable/text-input/text-input-unstable-v1.xml				\
	unstable/input-method/input-method-unstable-v1.xml			\
	unstable/wayland-tablet/wayland-tablet-unstable-v1.xml			\
	$(NULL)

nobase_dist_pkgdata_DATA = $(protocols)

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = wayland-protocols.pc

protocols_Server = $(protocols:.xml=-server-protocol.h)
protocols_Client = $(protocols:.xml=-client-protocol.h)

BUILT_SOURCES = 								\
	$(protocols_Server)							\
	$(protocols_Client)							\
	$(NULL)

%-server-protocol.h : $(top_srcdir)/%.xml
	$(AM_V_GEN)$(MKDIR_P) $(dir $@) && $(wayland_scanner) server-header < $< > $@

%-client-protocol.h : $(top_srcdir)/%.xml
	$(AM_V_GEN)$(MKDIR_P) $(dir $@) && $(wayland_scanner) client-header < $< > $@

alldirs := doc/Client doc/Server

$(alldirs):
	$(AM_V_GEN)$(MKDIR_P) $@

noinst_DATA = 									\
	doc/Client/index.html 							\
	doc/Server/index.html 							\
	$(NULL)

doc/%/index.html: $(protocols_%) wayland.doxygen | doc/%
	$(AM_V_GEN)(cat wayland.doxygen; \
          echo "GENERATE_HTML=YES"; \
          echo "HTML_OUTPUT=doc/$*"; \
          echo "INPUT= $(protocols_$*)"; \
          ) | $(DOXYGEN) -

clean:
	rm -f $(BUILT_SOURCES)
