include ../common/Makefile.common

all: fpga_lf.bit fpga_hf.bit
clean:
	$(DELETE) *.bgn *.drc *.ncd *.ngd *_par.xrpt *-placed.* *-placed_pad.* *_usage.xml xst_hf.srp xst_lf.srp
	$(DELETE) *.map *.ngc *.xrpt *.pcf *.rbt *_auto_* *.bld *.mrp *.ngm *.unroutes *_summary.xml netlist.lst xst

fpga_hf.ngc: fpga_hf.v fpga.ucf xst_hf.scr util.v hi_simulate.v hi_read_tx.v hi_read_rx_xcorr.v hi_iso14443a.v hi_sniffer.v hi_get_trace.v
	$(DELETE) $@
	$(XILINX_TOOLS_PREFIX)xst -ifn xst_hf.scr

fpga_lf.ngc: fpga_lf.v fpga.ucf xst_lf.scr util.v clk_divider.v lo_edge_detect.v lo_read.v lo_passthru.v lp20khz_1MSa_iir_filter.v min_max_tracker.v lf_edge_detect.v
	$(DELETE) $@
	$(XILINX_TOOLS_PREFIX)xst -ifn xst_lf.scr

%.ngd: %.ngc
	$(DELETE) $@
	$(XILINX_TOOLS_PREFIX)ngdbuild -aul -p xc2s30-5-vq100 -nt timestamp -uc fpga.ucf $< $@

%.ncd: %.ngd
	$(DELETE) $@
	$(XILINX_TOOLS_PREFIX)map -p xc2s30-5-vq100 $<

%-placed.ncd: %.ncd
	$(DELETE) $@
	$(XILINX_TOOLS_PREFIX)par $< $@

%.bit: %-placed.ncd
	$(DELETE) $@ $*.drc $*.rbt
	$(XILINX_TOOLS_PREFIX)bitgen $< $@

.PHONY: all clean help
help:
	@echo Multi-OS Makefile, you are running on $(DETECTED_OS)
	@echo Possible targets:
	@echo +	all   - Make fpga.bit, the FPGA bitstream 
	@echo +	clean - Clean intermediate files, does not clean fpga.bit
	
