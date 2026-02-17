export CROC_ROOT := $(shell cd .. && pwd)
export WORKDIR := $(shell pwd)

export PDK_ROOT=IHP-Open-PDK
export PDK=ihp-sg13g2

.PHONY: bender
bender: 
	bender \
    	script flist-plus \
        -t asic \
        -t ihp13 \
        -t rtl \
        -t synthesis \
        -D VERILATOR=1 \
        -D SYNTHESIS=1 \
        -D COMMON_CELLS_ASSERTS_OFF=1 \
        > src/croc.flist
	sed -i 's|${CROC_ROOT}|..|g' src/croc.flist

.PHONY: flist2yaml
flist2yaml: src/croc.flist
	python3 scripts/flist2jyaml.py src/croc.flist

.PHONY: librelane
librelane: 
	PDK_ROOT=$(WORKDIR)/$(PDK_ROOT) PDK=$(PDK) \
	librelane --manual-pdk --run-tag croc-soc --overwrite \
	config.yaml

.PHONY: librelane-skip-synth
librelane-skip-synth:
	PDK_ROOT=$(WORKDIR)/$(PDK_ROOT) PDK=$(PDK) \
	librelane --manual-pdk --run-tag croc-soc_no_synth --with-initial-state runs/croc-soc/06-yosys-synthesis/state_out.json --from Checker.YosysUnmappedCells --overwrite \
	config.yaml

.PHONY: view-results
view-results:
	PDK_ROOT=$(WORKDIR)/$(PDK_ROOT) PDK=$(PDK) \
	librelane --manual-pdk --last-run --flow OpenInOpenROAD \
	config.yaml