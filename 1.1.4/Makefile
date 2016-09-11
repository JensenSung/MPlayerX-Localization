
IBTOOL=/Applications/Xcode.app/Contents/Developer/usr/bin/ibtool

#this one must be ahead of EXPAND since FILES will be defined 
DESTROOT = results

define EXPAND

  FILES := $$(filter-out .git,$$(filter-out .svn, $$(filter-out English.lproj, $$(filter-out $(DESTROOT),$$(wildcard $(1)*)))))

  DIRS := 

  $$(foreach e, $$(FILES), $$(if $$(wildcard $$(e)/*), $$(eval DIRS := $$(DIRS) $$(e))))

  FILES := $$(filter-out $$(DIRS),$$(FILES))

  ALLFILES := $$(ALLFILES) $$(FILES)

  $$(foreach e,$$(DIRS),$$(eval $$(call EXPAND,$$(e)/)))

endef

$(eval $(call EXPAND))

############### Source of the xib and strings #################
XIBS = $(filter %.xib,$(ALLFILES))
STRS = $(filter %.strings,$(ALLFILES))
RTFS = $(filter %.rtf,$(ALLFILES))

### the compiled results of xib
NIBS = $(subst .xib,.nib,$(XIBS))

### There must be at least one strings file in the folder
### So it is safe to define the dir
LANG_DIRS = $(dir $(STRS))

############ the destination of every localization ############
DEST_DIRS = $(addprefix $(DESTROOT)/, $(LANG_DIRS))

TARGET_STRS = $(addprefix $(DESTROOT)/, $(STRS))
TARGET_NIBS = $(addprefix $(DESTROOT)/, $(NIBS))
TARGET_RTFS = $(addprefix $(DESTROOT)/, $(RTFS))

########################## Tragets #############################
all: $(TARGET_NIBS) $(TARGET_STRS) $(TARGET_RTFS)

.PHONY:clean
clean:
	rm -Rf $(DESTROOT)

############################# rules ############################
$(DESTROOT)/%.nib: %.xib $(DESTROOT)
#	cp "$<" "$@"
	${IBTOOL} --compile "$@" "$<"

$(DESTROOT)/%.strings: %.strings $(DESTROOT)
	cp "$<" "$@"

$(DESTROOT)/%.rtf: %.rtf $(DESTROOT)
	cp "$<" "$@"

$(DESTROOT):
	mkdir -p $(DEST_DIRS)