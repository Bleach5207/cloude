//wenguo
`ifndef test_second_register_after_speed_change_fulleq_SV
`define test_second_register_after_speed_change_fulleq_SV

class test_second_register_after_speed_change_fulleq extends test_pcie_eq_base;

    //---------------------------------------
    // Member
    //---------------------------------------
	typedef enum {
		cfg_gen4_usp,
		cfg_gen5_usp,
		cfg_gen3_eiec,
		cfg_tx_precode_req		
	} reg_type_e;


	bit [2:0]  group_bif_sel_a[PCIE_GROUP_NUM];
	int        error_count = 0;
	int        first_check;
    //---------------------------------------
    // Factory 
    //---------------------------------------
    `uvm_component_utils(test_second_register_after_speed_change_fulleq)

    //---------------------------------------
    // Constructor
    //---------------------------------------
    function new(string name = "test_second_register_after_speed_change_fulleq", uvm_component parent);
        super.new(name, parent);
		enable_group0 = 1;
    endfunction : new

    //---------------------------------------
    // Methods 
    //---------------------------------------
    extern virtual function void set_test_seq();
    extern virtual function void change_link_cfg();
	//else
	extern virtual task run_phase(uvm_phase phase);
	extern virtual task release_hdl_signals();
	extern virtual task check_register_usp(reg_type_e reg_type);
	extern virtual task check_register_dsp(int vip_idx, reg_type_e reg_type);
	extern virtual task automatic read_reg_32(input  bit [19:0] addr, output bit [31:0] rdata);
	extern virtual task automatic write_reg_32(input bit [19:0] addr, input bit [31:0] wdata);
	extern virtual function string get_dsp_hdl_path(int idx, reg_type_e reg_type);
	extern virtual function int  map_vip_idx_to_dsp_idx(int idx); 
	extern virtual function void dyn_change_work_mode_config();
	//extern virtual function void dyn_change_ip_config();

endclass : test_second_register_after_speed_change_fulleq
 
task automatic test_second_register_after_speed_change_fulleq::read_reg_32(input  bit [19:0] addr, output bit [31:0] rdata);
    bit [7:0] byte_data;
    rdata = '0;
    for (int k = 0; k < 4; k++) begin
        read_reg(I2C_DBI_4, addr + k, byte_data);
        rdata[8*k +: 8] = byte_data;
    end
endtask

task automatic test_second_register_after_speed_change_fulleq::write_reg_32(input bit [19:0] addr, input bit [31:0] wdata);
    
    for (int k = 0; k < 4; k++) begin
        write_reg_encap(I2C_DBI_4,addr + k,wdata[8*k +: 8]);
    end
endtask
task test_second_register_after_speed_change_fulleq::release_hdl_signals();
    string path;
    string usp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1d_core.u_ps9038_ip1d_pcie_sw.u0_sw_up_8.u_sw_up_g5x8_pcie_wrapper.u_sw_up_g5x8_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd";
    string usp_sigs[] = '{
        $sformatf("%s.u_cdm.u_cdm_pl32g_reg.cfg_tx_precode_req", usp_base),
        $sformatf("%s.u_smlh.u_smlh_eqctl.cfg_gen3_req_rst_eiec_disable", usp_base),
        $sformatf("%s.u_smlh.cfg_gen4_usp_send_8gt_eq_ts2_disable", usp_base),
        $sformatf("%s.u_smlh.cfg_gen5_usp_send_8gt_eq_ts2_disable", usp_base)
	};
	//usp
    foreach (usp_sigs[i]) begin
        uvm_hdl_release(usp_sigs[i]);
        `uvm_info("RELEASE", $sformatf("Released USP: %s", usp_sigs[i]), UVM_LOW)
    end
	//dsp
    for (int idx = 1; idx <= 19; idx++) begin
        string dsp_base;
        case (idx)
            1:  dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1p_dn.u_ps9038_ip1p_dn_pcie_sw.u0_sw_dn_hp_group_842.u0_sw_dn_g5x8_hp_pcie_wrapper.u_sw_dn_g5x8_hp_DWC_pcie_ctl";
            2:  dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1p_dn.u_ps9038_ip1p_dn_pcie_sw.u0_sw_dn_hp_group_842.u1_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl";
            3:  dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1p_dn.u_ps9038_ip1p_dn_pcie_sw.u0_sw_dn_hp_group_842.u2_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl";
            4:  dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u0_sw_dn_group_421.u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl";
            5:  dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u0_sw_dn_group_421.u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl";
            6:  dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u0_sw_dn_group_421.u2_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl";
            7:  dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u1_sw_dn_group_421.u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl";
            8:  dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u1_sw_dn_group_421.u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl";
            9:  dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u1_sw_dn_group_421.u2_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl";
            10: dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u2_sw_dn_group_4211.u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl";
            11: dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u2_sw_dn_group_4211.u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl";
            12: dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u2_sw_dn_group_4211.u2_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl";
            13: dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u2_sw_dn_group_4211.u3_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl";
            14: dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u0_sw_dn_group_421.u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl";
            15: dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u0_sw_dn_group_421.u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl";
            16: dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u0_sw_dn_group_421.u2_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl";
            17: dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u1_sw_dn_group_421.u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl";
            18: dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u1_sw_dn_group_421.u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl";
            19: dsp_base = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u1_sw_dn_group_421.u2_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl";
            default: dsp_base = "";
        endcase
				
        if (dsp_base != "") begin
            path = $sformatf("%s.u_cx_phy_logical.u_cx_phy_logical_swpd.u_cdm.u_cdm_pl32g_reg.cfg_tx_precode_req", dsp_base) ;
            uvm_hdl_release(path);
			
			path = $sformatf("%s.u_cx_phy_logical.u_cx_phy_logical_swpd.u_cdm.gen_cdm_reg[0].u_cdm_ecfg_reg.pl32g_reg_id[8:0]", dsp_base) ;
            uvm_hdl_release(path);

            path = $sformatf("%s.u_cx_phy_logical.u_cx_phy_logical_swpd.u_smlh.u_smlh_eqctl.cfg_gen3_req_rst_eiec_disable", dsp_base);
            uvm_hdl_release(path);

            `uvm_info("RELEASE", $sformatf("Released DSP[%0d] 2 signals", idx), UVM_LOW)
        end
    end
uvm_hdl_release("top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1d_core.u_ps9038_ip1d_pcie_sw.u0_sw_up_8.u_sw_up_g5x8_pcie_wrapper.u_sw_up_g5x8_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.u_cdm.u_cdm_pl_reg.pl_g4r_194[6:5]");
//uvm_hdl_release("top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1d_core.u_ps9038_ip1d_pcie_sw.u0_sw_up_8.u_sw_up_g5x8_pcie_wrapper.u_sw_up_g5x8_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.u_smlh.cfg_gen4_usp_send_8gt_eq_ts2_disable");

//uvm_hdl_release("top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1d_core.u_ps9038_ip1d_pcie_sw.u0_sw_up_8.u_sw_up_g5x8_pcie_wrapper.u_sw_up_g5x8_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.u_smlh.cfg_gen5_usp_send_8gt_eq_ts2_disable");
uvm_hdl_release("top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1d_core.u_ps9038_ip1d_pcie_sw.u0_sw_up_8.u_sw_up_g5x8_pcie_wrapper.u_sw_up_g5x8_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.u_cdm.u_cdm_pl_reg.pl_g5r_194");
uvm_hdl_release("top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1d_core.u_ps9038_ip1d_pcie_sw.u0_sw_up_8.u_sw_up_g5x8_pcie_wrapper.u_sw_up_g5x8_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.u_cdm.cfg_tx_precode_req");

uvm_hdl_release("top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1d_core.u_ps9038_ip1d_pcie_sw.u0_sw_up_8.u_sw_up_g5x8_pcie_wrapper.u_sw_up_g5x8_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.u_cdm.gen_cdm_reg[0].u_cdm_ecfg_reg.pl32g_reg_id[9:0]");

endtask : release_hdl_signals
//------------------------------------------
// run_phase
//------------------------------------------
task test_second_register_after_speed_change_fulleq::run_phase(uvm_phase phase);
	
	//parade_pcie_link_eq_cfg m_eq_cfg;
    bit        check_cfg_gen4_usp_send_8gt_eq_ts2_disable;
	bit        check_cfg_gen5_usp_send_8gt_eq_ts2_disable;
	bit        check_cfg_gen3_req_rst_eiec_disable;
	int        last_dsp_idx=-1;	

	//acl register
	bit [7:0]  rand_data[8][192];
	bit [19:0] reg_addr;
	bit [19:0] group_off[] = '{20'h94000};  
	int        group_lane_num_a[] = '{8};
	int        base_lane_addr = 20'h600;
	bit [7:0]  check_data;
	string reg_name;

	bit [31:0] addr;
	bit [31:0] rdata;
int sw_dn_pl32g_ptr_arr[$] = '{`SW_DN_G5X8_HP_PL32G_PTR, `SW_DN_G5X2_PL32G_PTR, `SW_DN_G5X4_PL32G_PTR,
								`SW_DN_G5X4_PL32G_PTR, `SW_DN_G5X1_PL32G_PTR, `SW_DN_G5X2_PL32G_PTR, 
								`SW_DN_G5X4_PL32G_PTR, `SW_DN_G5X1_PL32G_PTR, `SW_DN_G5X2_PL32G_PTR, 
								`SW_DN_G5X4_PL32G_PTR, `SW_DN_G5X1_PL32G_PTR, `SW_DN_G5X2_PL32G_PTR, `SW_DN_G5X1_PL32G_PTR, 
								`SW_DN_G5X4_PL32G_PTR, `SW_DN_G5X1_PL32G_PTR, `SW_DN_G5X2_PL32G_PTR,
								`SW_DN_G5X4_PL32G_PTR, `SW_DN_G5X1_PL32G_PTR, `SW_DN_G5X2_PL32G_PTR};



	dyn_change_work_mode_config();
`ifdef DBI_ADDR_WIDTH
    `uvm_info("DBI_ADDR_WIDTH", $sformatf("DBI_ADDR_WIDTH = %0d", `DBI_ADDR_WIDTH), UVM_LOW)
`else
    `uvm_info("DBI_ADDR_WIDTH", "DBI_ADDR_WIDTH is NOT defined", UVM_LOW)
`endif
	#200us;
	release_hdl_signals();

	//////////////////////////////////////////
	//Capability register
	//////////////////////////////////////////
			`uvm_info(get_name, $sformatf("start check usp"), UVM_LOW)
			addr = 20'h4000 + 20'h700 + 9'h190;
			/////////////////////////////////////////////
			//check cfg_gen4_usp_send_8gt_eq_ts2_disable
			/////////////////////////////////////////////
			read_reg_32(addr, rdata);
			rdata[26:24] = 3'b001;
			write_reg_32(addr, rdata);
			
			rdata[22] = 1'b1;

			write_reg_32(addr, rdata);
			check_register_usp(cfg_gen4_usp);
			`uvm_info("REG_WRITE", $sformatf("[%0t] Write addr = 32'h%08h, wdata = 32'h%08h", $time, addr, rdata), UVM_LOW)

			/////////////////////////////////////////////
			//check cfg_gen5_usp_send_8gt_eq_ts2_disable
			/////////////////////////////////////////////

			read_reg_32(addr, rdata);
			rdata[26:24] = 3'b010;
			write_reg_32(addr, rdata);
			
			rdata[22] = 1'b1;    // cfg_gen5_usp_send_16gt_eq_ts2_disable
			write_reg_32(addr, rdata);
			
			check_register_usp(cfg_gen5_usp);
			`uvm_info("REG_WRITE",$sformatf("[%0t] Write addr = 32'h%08h, wdata = 32'h%08h",$time, addr, rdata), UVM_LOW)
			
			/////////////////////////////////////////////
			// check cfg_gen3_req_rst_eiec_disable usp just one core
			/////////////////////////////////////////////
			read_reg_32(addr, rdata);
			
			rdata[10] = 1'b1;    // cfg_gen3_req_rst_eiec_disable
			write_reg_32(addr, rdata);

			check_register_usp(cfg_gen3_eiec);
			`uvm_info("REG_WRITE",$sformatf("[%0t] Write addr = 32'h%08h, wdata = 32'h%08h",$time, addr, rdata), UVM_LOW)
			///////////////////////////////////////
			////check cfg_tx_precode_req usp just one core
			///////////////////////////////////////
			////////////////en
				addr = 20'h4000 + 20'h700 + 20'h1bc;
				read_reg_32(addr, rdata);
				rdata[0] = 1'b1;
				write_reg_32(addr, rdata);
			///////////
				addr = 20'h4000 + 20'h1c8 + 20'h00c;
				read_reg_32(addr, rdata);
				rdata[9] = 1'b1;
				write_reg_32(addr, rdata);
				
				first_check = 1;
			check_register_usp(cfg_tx_precode_req);
										
				rdata[9] = 1'b1;
				write_reg_32(addr, rdata);
				first_check = 0;
			check_register_usp(cfg_tx_precode_req);


		for(int  sw_dev_idx= 0; sw_dev_idx < 28; sw_dev_idx++)begin
			///////////////////////////////////////
			//check cfg_gen3_req_rst_eiec_disable,               sw_dev_idx 0-27 infer to lanes,map_vip_idx_to_dsp_idx(sw_dev_idx) 1-19 infer to cores;
			///////////////////////////////////////
			`uvm_info(get_name, $sformatf("start check DSP"), UVM_LOW)

			if (map_vip_idx_to_dsp_idx(sw_dev_idx) != last_dsp_idx) begin
				last_dsp_idx = map_vip_idx_to_dsp_idx(sw_dev_idx);

				///////////////////////////////////////
				//check cfg_gen3_req_rst_eiec_disable
				///////////////////////////////////////
				addr = 20'h4000 + map_vip_idx_to_dsp_idx(sw_dev_idx) * 20'h1000 + 20'h700 + 20'h190;
				if (map_vip_idx_to_dsp_idx(sw_dev_idx) > 11) begin
					addr += 20'h4000;
				end
				read_reg_32(addr, rdata);
				rdata[10] = 1'b1;
				write_reg_32(addr, rdata);

				///////////////////////////////////////
				//write cfg_tx_precode_req
				///////////////////////////////////////
				addr = 20'h4000 + map_vip_idx_to_dsp_idx(sw_dev_idx) * 20'h1000 + 20'h700 + 20'h1bc;////////SW_DN_G*X*_CFG_PL_REG
				if (map_vip_idx_to_dsp_idx(sw_dev_idx) > 11) begin
					addr += 20'h4000;
				end
				read_reg_32(addr, rdata);
				rdata[0] = 1'b1;
				write_reg_32(addr, rdata);

				addr = 20'h4000 + map_vip_idx_to_dsp_idx(sw_dev_idx) * 20'h1000 + sw_dn_pl32g_ptr_arr[map_vip_idx_to_dsp_idx(sw_dev_idx)-1] + 20'h00c;////////SW_DN_G*X*_PL32G_PTR
				if (map_vip_idx_to_dsp_idx(sw_dev_idx) > 11) begin
					addr += 20'h4000;
				end
				read_reg_32(addr, rdata);
				rdata[9] = 1'b1;
				write_reg_32(addr, rdata);
				//Check cfg_tx_precode_req,check all core 1-20 for dsp
				first_check = 1;
				check_register_dsp(map_vip_idx_to_dsp_idx(sw_dev_idx),cfg_tx_precode_req);
				
				rdata[9] = 1'b1;
				write_reg_32(addr, rdata);
				first_check = 0;
				check_register_dsp(map_vip_idx_to_dsp_idx(sw_dev_idx),cfg_tx_precode_req);


			end
			//Check cfg_gen3_req_rst_eiec_disable,check all lane 1-28 for dsp
			check_register_dsp(sw_dev_idx,cfg_gen3_eiec);
		
		if(sw_dev_idx >= 0)begin
			`uvm_info(get_name, $sformatf("hello0, com.idx = %0d, dsp_idx is %0d",sw_dev_idx,  map_vip_idx_to_dsp_idx(sw_dev_idx)), UVM_LOW)
		end else begin
			`uvm_info(get_name, $sformatf("hello0"), UVM_LOW)
		end
		`uvm_info(get_name, $sformatf("hello1, com.idx = %0d, addr is %h", sw_dev_idx, addr), UVM_LOW)
		end

	//////////////////////////////////////////
	//ACL register
	//////////////////////////////////////////
	for(int i=0; i<group_off.size(); i++)begin
		for(int j=0; j<group_lane_num_a[i];j++)begin
			reg_addr =  group_off[i]+(base_lane_addr + j*20'h400);
			// REG_00 ~ REG_8F
			for (int k = 0; k < 144; k++) begin
				rand_data[j][k] = $urandom_range(8'hFF, 8'h00);
				write_reg_encap(I2C_DBI_4, reg_addr + k, rand_data[j][k]);
			end

			// REG_D0 ~ REG_FF
			for (int k = 0; k < 48; k++) begin
				rand_data[j][144+k] = $urandom_range(8'hFF, 8'h00);
				write_reg_encap(I2C_DBI_4, reg_addr + 20'hD0 + k,rand_data[j][144+k]);
			end

			// check REG_00 ~ REG_8F
			for (int k = 0; k < 144; k++) begin
			    reg_name = $sformatf("%02h", k);
			    reg_name = reg_name.toupper();
			
			    uvm_hdl_read($sformatf("top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1p_up_phyd.U_PXC_PRX_ACL_NCH.U_PRX_CH[%0d].ACL_TOP.ACL_TOP_REG0.REG_%s[7:0]", j, reg_name), check_data);
			
			    if (check_data !== rand_data[j][k]) begin
			        error_count++;
			        `uvm_error("REG_CHECK",$sformatf("Lane%0d REG_%s: exp=0x%02h act=0x%02h",j,reg_name,rand_data[j][k],check_data))
			    end
				else begin
					`uvm_info("ACL_check_PASS",$sformatf("PASSED, Lane%0d REG_%s: exp=0x%02h act=0x%02h",j,reg_name,rand_data[j][k],check_data), UVM_LOW)
				end
			end
			
			// check REG_D0 ~ REG_FF
			for (int k = 0; k < 48; k++) begin
			    reg_name = $sformatf("%02h", 8'hD0 + k);
			    reg_name = reg_name.toupper();
			
			    uvm_hdl_read($sformatf("top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1p_up_phyd.U_PXC_PRX_ACL_NCH.U_PRX_CH[%0d].ACL_TOP.ACL_TOP_REG0.REG_%s[7:0]", j, reg_name), check_data);
			
			    if (check_data !== rand_data[j][144+k]) begin
			        error_count++;
			        `uvm_error("REG_CHECK",$sformatf("Lane%0d REG_%s: exp=0x%02h act=0x%02h",j,reg_name,rand_data[j][144+k],check_data))
			    end
				/*
				else begin
					`uvm_info("ACL_check_PASS",$sformatf("PASSED, Lane%0d REG_%s: exp=0x%02h act=0x%02h",j,reg_name,rand_data[j][144+k],check_data), UVM_LOW)
				end
				*/

			end
		end
	end

	if (error_count == 0) begin
		`uvm_info("CTLE_CHECK","CTLE CHECK PASSED! No errors found.",UVM_LOW)
	end
	else begin
		`uvm_error("CTLE_CHECK",$sformatf("CTLE CHECK FAILED! Total error count = %0d", error_count))
	end

endtask : run_phase

function string test_second_register_after_speed_change_fulleq::get_dsp_hdl_path(int idx, reg_type_e reg_type);
    string group_base_path, wrapper_prefix,full_path;
    int group_id, rel_idx, slv_idx;
    bit [2:0] bif_sel;

    if (idx < 0 || idx > 27) return "";

//////////////////////////////////////////////////
/////////cfg_tx_precode_req
//////////////////////////////////////////////////
	if (reg_type == cfg_tx_precode_req) begin
        case (idx)
            // Group 0 (dsp 1~3)
            1:  full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1p_dn.u_ps9038_ip1p_dn_pcie_sw.u0_sw_dn_hp_group_842.u0_sw_dn_g5x8_hp_pcie_wrapper.u_sw_dn_g5x8_hp_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            2:  full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1p_dn.u_ps9038_ip1p_dn_pcie_sw.u0_sw_dn_hp_group_842.u1_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            3:  full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1p_dn.u_ps9038_ip1p_dn_pcie_sw.u0_sw_dn_hp_group_842.u2_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            // Group 1 (dsp 4~6)
            4:  full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u0_sw_dn_group_421.u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            5:  full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u0_sw_dn_group_421.u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            6:  full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u0_sw_dn_group_421.u2_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.u_cdm.cfg_tx_precode_req";
            // Group 2 (dsp 7~9)
            7:  full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u1_sw_dn_group_421.u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            8:  full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u1_sw_dn_group_421.u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            9:  full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u1_sw_dn_group_421.u2_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            // Group 3 (dsp 10~13)
            10: full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u2_sw_dn_group_4211.u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            11: full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u2_sw_dn_group_4211.u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            12: full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u2_sw_dn_group_4211.u2_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            13: full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u2_sw_dn_group_4211.u3_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            // Group 4 (dsp 14~16)
            14: full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u0_sw_dn_group_421.u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            15: full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u0_sw_dn_group_421.u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            16: full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u0_sw_dn_group_421.u2_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            // Group 5 (dsp 17~19)
            17: full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u1_sw_dn_group_421.u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            18: full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u1_sw_dn_group_421.u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            19: full_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u1_sw_dn_group_421.u2_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.cfg_tx_precode_req";
            default: return "";
        endcase
        return full_path;
    end
/////////////////////////////////////////////////////
////////
////////////////////////////////////////////////////
    idx = idx + 1;
	if (idx <= 8) begin
        group_id = 0;
        rel_idx  = idx - 1;
        group_base_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1p_dn.u_ps9038_ip1p_dn_pcie_sw.u0_sw_dn_hp_group_842";
    end else begin
        group_id = (idx <= 12) ? 1 : (idx <= 16) ? 2 : (idx <= 20) ? 3 : (idx <= 24) ? 4 : 5;
        rel_idx  = (idx - 9) % 4; 
        case (group_id)
            1: group_base_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u0_sw_dn_group_421";
            2: group_base_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u1_sw_dn_group_421";
            3: group_base_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP2_TOP.PS9038_IP2_CORE.PS9038_IP2D_CORE.ps9038_ip2d_pcie_sw.u2_sw_dn_group_4211";
            4: group_base_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u0_sw_dn_group_421";
            5: group_base_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP3_TOP.PS9038_IP3D_CORE.u_ps9038_ip3d_pcie_sw.u1_sw_dn_group_421";
        endcase
    end

    bif_sel = group_bif_sel_a[group_id];

    if (group_id == 0) begin 
        string w_g8 = "u0_sw_dn_g5x8_hp_pcie_wrapper.u_sw_dn_g5x8_hp_DWC_pcie_ctl";
        string w_g2 = "u1_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl";
        string w_g4 = "u2_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl";
        case (bif_sel)
            3'b000: begin wrapper_prefix = w_g8; slv_idx = rel_idx; end
            3'b001: begin wrapper_prefix = (rel_idx <= 3) ? w_g8 : w_g4; slv_idx = (rel_idx <= 3) ? rel_idx : rel_idx - 4; end
            3'b010: begin wrapper_prefix = (rel_idx <= 3) ? w_g8 : (rel_idx <= 5) ? w_g2 : w_g4; slv_idx = (rel_idx <= 3) ? rel_idx : (rel_idx <= 5) ? rel_idx - 4 : rel_idx - 6; end
            3'b011: begin wrapper_prefix = (rel_idx <= 1) ? w_g8 : (rel_idx <= 3) ? w_g2 : w_g4; slv_idx = (rel_idx <= 1) ? rel_idx : (rel_idx <= 3) ? rel_idx - 2 : rel_idx - 4; end
            default: return "";
        endcase
    end else begin 
        
        string w_x4   = "u0_sw_dn_g5x4_pcie_wrapper.u_sw_dn_g5x4_DWC_pcie_ctl";
        string w_x1_a = "u1_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl";
        string w_x1_b = (group_id == 3) ? "u2_sw_dn_g5x1_pcie_wrapper.u_sw_dn_g5x1_DWC_pcie_ctl" : w_x1_a;
        string w_x2   = (group_id == 3) ? "u3_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl" : "u2_sw_dn_g5x2_pcie_wrapper.u_sw_dn_g5x2_DWC_pcie_ctl";

        case (bif_sel)
            3'b000: begin wrapper_prefix = w_x4; slv_idx = rel_idx; end
            3'b001: begin wrapper_prefix = (rel_idx <= 1) ? w_x4 : w_x2; slv_idx = (rel_idx <= 1) ? rel_idx : rel_idx - 2; end
			3'b010: begin wrapper_prefix = (rel_idx <= 1) ? w_x4 : (rel_idx == 2) ? w_x1_b : w_x2 ; slv_idx = (rel_idx <= 1) ? rel_idx : 0; end
            3'b011: begin wrapper_prefix = (rel_idx == 0) ? w_x4 : (rel_idx == 1) ? w_x1_a : w_x2; slv_idx = (rel_idx <= 1) ? 0 : rel_idx - 2; end
            3'b100: begin
                if (rel_idx == 3 && group_id != 3) return ""; 
                wrapper_prefix = (rel_idx == 0) ? w_x4 : (rel_idx == 1) ? w_x1_a : (rel_idx == 2) ? ((group_id == 3) ? w_x1_b : w_x2) : w_x2;
                slv_idx = 0;
            end
            default: return "";
        endcase
    end

    return $sformatf("%s.%s.u_cx_phy_logical.u_cx_phy_logical_swpd.u_smlh.u_smlh_eqpa.u_smlh_eqpa_slv[%0d].cfg_gen3_req_rst_eiec_disable",
                     group_base_path, wrapper_prefix, slv_idx);
endfunction

task test_second_register_after_speed_change_fulleq::check_register_dsp(int vip_idx, reg_type_e reg_type);
    string hdl_path;
    bit    check_data;
	bit    expected_data;

    hdl_path = get_dsp_hdl_path(vip_idx, reg_type);

    if (hdl_path == "") begin
        `uvm_info("CHECK_REG_DSP", $sformatf("vip_idx=%0d is unmapped under current mode, skipping check.", vip_idx), UVM_HIGH)
        return;
    end

    uvm_hdl_read(hdl_path, check_data);

if (reg_type == cfg_tx_precode_req) begin
    if (first_check)
        expected_data = 1'b1;
    else
        expected_data = 1'b0;
end
else begin
    expected_data = 1'b1;
end

    if (check_data !== expected_data) begin
        error_count++;
		if (reg_type == cfg_tx_precode_req) 
			`uvm_error("CHECK_REG_DSP", $sformatf("core_idx=%0d reg=%0s CHECK FAILED! path=%s, actual=%0d",vip_idx, reg_type.name(), hdl_path, check_data))
		else if(reg_type == cfg_gen3_eiec) 
			`uvm_error("CHECK_REG_DSP", $sformatf("lane_idx=%0d reg=%0s CHECK FAILED! path=%s, actual=%0d",vip_idx, reg_type.name(), hdl_path, check_data))
    end
    else begin
		if (reg_type == cfg_tx_precode_req)
			`uvm_info("CHECK_REG_DSP", $sformatf("core_idx=%0d reg=%0s CHECK PASSED! path=%s", vip_idx, reg_type.name(), hdl_path), UVM_LOW)
		else if(reg_type == cfg_gen3_eiec)
			`uvm_info("CHECK_REG_DSP", $sformatf("lane_idx=%0d reg=%0s CHECK PASSED! path=%s", vip_idx, reg_type.name(), hdl_path), UVM_LOW)
    end

endtask


task test_second_register_after_speed_change_fulleq::check_register_usp(reg_type_e reg_type);

    string base_path = "top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1d_core.u_ps9038_ip1d_pcie_sw.u0_sw_up_8.u_sw_up_g5x8_pcie_wrapper.u_sw_up_g5x8_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.u_smlh";
    string paths[$]; 
    bit    check_data;
	bit    expected_data;

    case (reg_type)
        cfg_gen4_usp:  paths.push_back($sformatf("%s.cfg_gen4_usp_send_8gt_eq_ts2_disable", base_path));
        cfg_gen5_usp:  paths.push_back($sformatf("%s.cfg_gen5_usp_send_8gt_eq_ts2_disable", base_path));
        cfg_gen3_eiec: begin
							for (int i = 0; i < 8; i++) begin
								paths.push_back($sformatf("%s.u_smlh_eqpa.u_smlh_eqpa_slv[%0d].cfg_gen3_req_rst_eiec_disable", base_path, i));
							end
					   end
		cfg_tx_precode_req: paths.push_back("top.u_wrapper_of_chip.PS9038.PS9038_IP1_TOP.u_ps9038_ip1_core.u_ps9038_ip1d_core.u_ps9038_ip1d_pcie_sw.u0_sw_up_8.u_sw_up_g5x8_pcie_wrapper.u_sw_up_g5x8_DWC_pcie_ctl.u_cx_phy_logical.u_cx_phy_logical_swpd.u_cdm.cfg_tx_precode_req");

        default: begin
            `uvm_error("CHECK_REGISTER", $sformatf("Unsupported reg_type: %0s", reg_type.name()))
            return;
        end
    endcase

if (reg_type == cfg_tx_precode_req) begin
    if (first_check)
        expected_data = 1'b1;
    else
        expected_data = 1'b0;
end
else begin
    expected_data = 1'b1;
end

    foreach (paths[i]) begin
        uvm_hdl_read(paths[i], check_data);

        if (check_data !== expected_data) begin
            error_count++;
            `uvm_error("CHECK_REGISTER", $sformatf("reg=%0s [%0d/%0d] check failed, path=%s, actual=%0d", 
                       reg_type.name(), i, paths.size(), paths[i], check_data))
        end
        else begin
            `uvm_info("CHECK_REGISTER", $sformatf("reg=%0s [%0d/%0d] check pass, path=%s", 
                      reg_type.name(), i, paths.size(), paths[i]), UVM_LOW)
        end
    end

endtask


//------------------------------------------
//  map_vip_idx_to_dsp_idx
//------------------------------------------
function int test_second_register_after_speed_change_fulleq::map_vip_idx_to_dsp_idx(int idx);
	static int dsp_idx = 1;
    int lane;

    `ifdef PS9038
    if (idx >= 0 && idx <= 7) begin
        case (group_bif_sel_a[0])
            3'b000: dsp_idx = 1;
            3'b001: dsp_idx = (idx <= 3) ? 1 : 3;
			3'b010: dsp_idx = (idx <= 3) ? 1 : ((idx <= 5) ? 2 : 3);
			3'b011: dsp_idx = (idx <= 1) ? 1 : ((idx <= 3) ? 2 : 3);
        endcase
    end
    
    else if (idx >= 16 && idx <= 19) begin
        lane = idx - 16;
        case (group_bif_sel_a[3])
            3'b000: dsp_idx = 10;
            3'b001: dsp_idx = (lane < 2) ? 10 : 12;
            3'b010: dsp_idx = (lane < 2) ? 10 : ((lane == 2) ? 11 : 12);
            3'b011: dsp_idx = (lane == 0) ? 10 : ((lane == 1) ? 11 : 12);
            3'b100: dsp_idx = 10 + lane;
        endcase
    end

    else if ((idx >= 8 && idx <= 15) || (idx >= 20 && idx <= 27)) begin
        int g_id     = (idx < 16) ? ((idx - 8) / 4 + 1) : ((idx - 20) / 4 + 4); 
        int base_dsp = (g_id == 1) ? 4 : (g_id == 2) ? 7 : (g_id == 4) ? 14 : 17; 
        lane         = (idx < 16) ? (idx % 4) : ((idx - 20) % 4);

        case (group_bif_sel_a[g_id])
            3'b000: dsp_idx = base_dsp;
            3'b001: dsp_idx = (lane < 2) ? base_dsp : (base_dsp + 2);
            3'b010: dsp_idx = (lane < 2) ? base_dsp : ((lane == 2) ? (base_dsp + 1) : (base_dsp + 2));
            3'b011: dsp_idx = (lane == 0) ? base_dsp : ((lane == 1) ? (base_dsp + 1) : (base_dsp + 2));
            3'b100: if (lane < 3) dsp_idx = base_dsp + lane; 
        endcase
    end
    `else

    `endif

    return dsp_idx;
endfunction

/*


function int test_second_register_after_speed_change_fulleq:: map_vip_idx_to_dsp_idx(int idx);
	//int dsp_idx;
	static int dsp_idx = 1;
	int reduced_idx;
	`ifdef PS9038
	if(idx >= 0 && idx <= 7)begin//group0
		if(idx == 0)
			dsp_idx = 1;
		if(idx == 2 || idx == 4 && group_bif_sel_a[0]  == 3'b010)
			dsp_idx = 2;	
		if(idx == 6 || idx == 4 && (group_bif_sel_a[0]  == 3'b011 || group_bif_sel_a[0]  == 3'b001))
			dsp_idx = 3;		
	end else if(idx >= 8 && idx <= 11)begin//group1
		reduced_idx = idx - 8;
		if(reduced_idx == 0)
			dsp_idx = 4;
		if(reduced_idx == 1 || reduced_idx == 2 && group_bif_sel_a[1]  == 3'b010)
			dsp_idx = 5;
		if(reduced_idx == 2 && (group_bif_sel_a[1]  == 3'b001 || group_bif_sel_a[1]  == 3'b011 || group_bif_sel_a[1]  == 3'b100) || reduced_idx == 3)
			dsp_idx = 6;				
	end else if(idx >= 12 && idx <= 15)begin//group2
		reduced_idx = idx - 12;		
		if(reduced_idx == 0)
			dsp_idx = 7;
		if(reduced_idx == 1 || reduced_idx == 2 && group_bif_sel_a[2]  == 3'b010)
			dsp_idx = 8;
		if(reduced_idx == 2 && (group_bif_sel_a[2]  == 3'b001 || group_bif_sel_a[2]  == 3'b011 || group_bif_sel_a[2]  == 3'b100) || reduced_idx == 3)
			dsp_idx = 9;				
	end else if(idx >= 16 && idx <= 19)begin//group3
        //Bifu      EP index                        core num
        //0000      EP[16]                          SW10
        //0001      EP[16],EP[18]                   SW10, SW13
        //0010      EP[16],EP[18],EP[19]            SW10, SW11, SW13
        //0011      EP[16],EP[17],EP[19]            SW10, SW11, SW13
        //0100      EP[16],EP[17],EP[18],EP[19]     SW10, SW11, SW13,SW12
		reduced_idx = idx - 16;
		if(reduced_idx == 0)
			dsp_idx = 10;
		if(reduced_idx == 1)
			dsp_idx = 11;
		if(reduced_idx == 2)//this should be connection to X2 core. RTL core seq is 4112  
			dsp_idx = group_bif_sel_a[3]==3'b010 ? 11 : 13;	
		if(reduced_idx == 3)
			dsp_idx = group_bif_sel_a[3]==3'b100 ? 12 : 13;
	end else if(idx >= 20 && idx <= 23)begin//group4
		reduced_idx = idx - 20;
		if(reduced_idx == 0)
			dsp_idx = 14;
		if(reduced_idx == 1 || reduced_idx == 2 && group_bif_sel_a[4]  == 3'b010)
			dsp_idx = 15;
		if(reduced_idx == 2 && (group_bif_sel_a[4]  == 3'b001 || group_bif_sel_a[4]  == 3'b011 ||  group_bif_sel_a[4]  == 3'b100) || reduced_idx == 3)
			dsp_idx = 16;				
	end else if(idx >= 24 && idx <= 27)begin//group5
		reduced_idx = idx - 24;			
		if(reduced_idx == 0)
			dsp_idx = 17;
		if(reduced_idx == 1 || reduced_idx == 2 && group_bif_sel_a[5]  == 3'b010)
			dsp_idx = 18;
		if(reduced_idx == 2 && (group_bif_sel_a[5]  == 3'b001 || group_bif_sel_a[5]  == 3'b011 || group_bif_sel_a[5]  == 3'b100) || reduced_idx == 3)
			dsp_idx = 19;				
		//`uvm_info("group5", $sformatf("reduced_idx is %0d, group_bif_sel_a[5] is %0b", reduced_idx, group_bif_sel_a[5]), UVM_LOW)
	end
	`else
	if(idx >= 0 && idx <= 3)begin//group0
		if(idx == 0)
			dsp_idx = 1;
		if(idx == 1 || idx == 2 && group_bif_sel_a[0]  == 3'b010)
			dsp_idx = 2;	
		if(idx == 3 || idx == 2 && (group_bif_sel_a[0]  == 3'b011 || group_bif_sel_a[0]  == 3'b001))
			dsp_idx = 3;		
	end else if(idx >= 4 && idx <= 7)begin//group1
		reduced_idx = idx - 4;
		if(reduced_idx == 0)
			dsp_idx = 4;
		if(reduced_idx == 1 || reduced_idx == 2 && group_bif_sel_a[1]  == 3'b010)
			dsp_idx = 5;
		if((reduced_idx == 2 && (group_bif_sel_a[1]  == 3'b001 || group_bif_sel_a[1] == 3'b011 || group_bif_sel_a[1] == 3'b100)) || reduced_idx == 3)
			dsp_idx = 6;	
	end else if(idx >= 8 && idx <= 11)begin//group3
		reduced_idx = idx - 8;
		if(reduced_idx == 0)
			dsp_idx = 7;
		if(reduced_idx == 1 || reduced_idx == 2 && group_bif_sel_a[2]==3'b010)
			dsp_idx = 8;
		if(reduced_idx == 2 && (group_bif_sel_a[2]==3'b001 || group_bif_sel_a[2]==3'b011 || group_bif_sel_a[2]==3'b100) || reduced_idx == 3 && group_bif_sel_a[2]==3'b010) 
			dsp_idx =  9;	
		if(reduced_idx == 3 && group_bif_sel_a[2]==3'b100)
			dsp_idx = 10;
	end
	`endif
	return dsp_idx;
endfunction 

function void test_second_register_after_speed_change_fulleq::dyn_change_ip_config();
//////////////////////////////////////////////////////////
    m_chip_cfg.m_pcie_component_vip_cfg[0].m_link_cfg.set_speed(5,5);
    m_chip_cfg.m_pcie_component_vip_cfg[0].m_link_cfg.lane_num = 4;
    m_chip_cfg.m_pcie_component_vip_cfg[4].m_link_cfg.set_speed(5,5);
    m_chip_cfg.m_pcie_component_vip_cfg[4].m_link_cfg.lane_num = 4;
    m_chip_cfg.m_pcie_component_vip_cfg[6].m_link_cfg.set_speed(5,5);
    m_chip_cfg.m_pcie_component_vip_cfg[6].m_link_cfg.lane_num = 4;

    m_chip_cfg.m_pcie_component_vip_cfg[8].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[8].m_link_cfg.lane_num = 2;
	m_chip_cfg.m_pcie_component_vip_cfg[10].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[10].m_link_cfg.lane_num = 1;
	m_chip_cfg.m_pcie_component_vip_cfg[11].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[11].m_link_cfg.lane_num = 1;

    m_chip_cfg.m_pcie_component_vip_cfg[12].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[12].m_link_cfg.lane_num = 2;
	m_chip_cfg.m_pcie_component_vip_cfg[14].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[14].m_link_cfg.lane_num = 1;
	m_chip_cfg.m_pcie_component_vip_cfg[15].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[15].m_link_cfg.lane_num = 1;

    m_chip_cfg.m_pcie_component_vip_cfg[16].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[16].m_link_cfg.lane_num = 2;
	m_chip_cfg.m_pcie_component_vip_cfg[18].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[18].m_link_cfg.lane_num = 1;
	m_chip_cfg.m_pcie_component_vip_cfg[19].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[19].m_link_cfg.lane_num = 1;

    m_chip_cfg.m_pcie_component_vip_cfg[20].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[20].m_link_cfg.lane_num = 2;
	m_chip_cfg.m_pcie_component_vip_cfg[22].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[22].m_link_cfg.lane_num = 1;
	m_chip_cfg.m_pcie_component_vip_cfg[23].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[23].m_link_cfg.lane_num = 1;

    m_chip_cfg.m_pcie_component_vip_cfg[24].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[24].m_link_cfg.lane_num = 2;
	m_chip_cfg.m_pcie_component_vip_cfg[26].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[26].m_link_cfg.lane_num = 1;
	m_chip_cfg.m_pcie_component_vip_cfg[27].m_link_cfg.set_speed(4,4);
    m_chip_cfg.m_pcie_component_vip_cfg[27].m_link_cfg.lane_num = 1;


	m_chip_cfg.m_pcie_component_vip_cfg[0].m_link_cfg.set_eq_mode(NO_EQ);
	m_chip_cfg.m_pcie_component_vip_cfg[4].m_link_cfg.set_eq_mode(NO_EQ);
	m_chip_cfg.m_pcie_component_vip_cfg[6].m_link_cfg.set_eq_mode(NO_EQ);

    m_work_mode_cfg.cml_pcie_g5x4_num = 1; 
    m_work_mode_cfg.cml_pcie_g5x4_num_big_than_idx = 0; 
	m_work_mode_cfg.cml_pcie_g5x2_num = 2; 
    m_work_mode_cfg.cml_pcie_g5x2_num_big_than_idx = 0; 

    m_work_mode_cfg.cml_pcie_g4x2_num = 5; 
    m_work_mode_cfg.cml_pcie_g4x2_num_big_than_idx = 0; 
    m_work_mode_cfg.cml_pcie_g4x1_num = 10; 
    m_work_mode_cfg.cml_pcie_g4x1_num_big_than_idx = 0; 

`uvm_info("010","buffercation",UVM_LOW)
endfunction
*/

function void test_second_register_after_speed_change_fulleq::dyn_change_work_mode_config();/*{{{*/
    int loop_j,loop_i;
/*
	m_work_mode_cfg.cml_pcie_g5x8_idx_a.push_back(0);
    m_work_mode_cfg.cml_pcie_g4x4_idx_a.push_back(8); 
    m_work_mode_cfg.cml_pcie_g4x4_idx_a.push_back(12); 
    m_work_mode_cfg.cml_pcie_g4x4_idx_a.push_back(16); 
    m_work_mode_cfg.cml_pcie_g4x4_idx_a.push_back(20);
    m_work_mode_cfg.cml_pcie_g4x4_idx_a.push_back(24);
*/
    m_work_mode_cfg.cml_pcie_g5x4_idx_a.push_back(0); 
    m_work_mode_cfg.cml_pcie_g5x2_idx_a.push_back(4);
	m_work_mode_cfg.cml_pcie_g5x2_idx_a.push_back(6);
	m_work_mode_cfg.cml_pcie_g4x2_idx_a.push_back(8);
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(10); 
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(11);

	m_work_mode_cfg.cml_pcie_g4x2_idx_a.push_back(12);
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(14); 
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(15);

	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(16);
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(17);
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(18); 
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(19);

	m_work_mode_cfg.cml_pcie_g4x2_idx_a.push_back(20);
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(22); 
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(23);

	m_work_mode_cfg.cml_pcie_g4x2_idx_a.push_back(24);
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(26); 
	m_work_mode_cfg.cml_pcie_g4x1_idx_a.push_back(27);



///define buffercation/*{{{*/
//////////////////////////////////
`ifdef PS9038
    // group 0
    if( m_work_mode_cfg.e_gpp_type_array[0] == GPP_PCIE_G5X8_0 )
        group_bif_sel_a[0] = 3'b000;
    else if( m_work_mode_cfg.e_gpp_type_array[0] == GPP_PCIE_G5X4_0 & m_work_mode_cfg.e_gpp_type_array[4] == GPP_PCIE_G5X4_0 )
        group_bif_sel_a[0] = 3'b001;
    else if( m_work_mode_cfg.e_gpp_type_array[0] == GPP_PCIE_G5X4_0 & m_work_mode_cfg.e_gpp_type_array[4] == GPP_PCIE_G5X2_0 & m_work_mode_cfg.e_gpp_type_array[6] == GPP_PCIE_G5X2_0 )
        group_bif_sel_a[0] = 3'b010;
    else if( m_work_mode_cfg.e_gpp_type_array[0] == GPP_PCIE_G5X2_0 & m_work_mode_cfg.e_gpp_type_array[2] == GPP_PCIE_G5X2_0 & m_work_mode_cfg.e_gpp_type_array[4] == GPP_PCIE_G5X4_0 )
        group_bif_sel_a[0] = 3'b011;
    else 
        group_bif_sel_a[0] = 3'b000; // should not go this else
`endif
    
    // other group 9038 1~5 / 9035 1~2
    for(loop_i=1; loop_i<PCIE_GROUP_NUM; loop_i++) begin
        loop_j = PCIE_GROUP_START_IDX[loop_i];
        if( m_work_mode_cfg.e_gpp_type_array[loop_j] == GPP_PCIE_G4X4_0) 
            group_bif_sel_a[loop_i] = 3'b000;
        else if(m_work_mode_cfg.e_gpp_type_array[loop_j] == GPP_PCIE_G4X2_0 & m_work_mode_cfg.e_gpp_type_array[loop_j+2] == GPP_PCIE_G4X2_0)
            group_bif_sel_a[loop_i] = 3'b001;
        else if(m_work_mode_cfg.e_gpp_type_array[loop_j] == GPP_PCIE_G4X2_0 & m_work_mode_cfg.e_gpp_type_array[loop_j+2] == GPP_PCIE_G4X1_0)
            group_bif_sel_a[loop_i] = 3'b010;
        else if(m_work_mode_cfg.e_gpp_type_array[loop_j] == GPP_PCIE_G4X1_0 & m_work_mode_cfg.e_gpp_type_array[loop_j+2] == GPP_PCIE_G4X2_0)
            group_bif_sel_a[loop_i] = 3'b011;
        else if(m_work_mode_cfg.e_gpp_type_array[loop_j] == GPP_PCIE_G4X1_0 & m_work_mode_cfg.e_gpp_type_array[loop_j+2] == GPP_PCIE_G4X1_0)
            group_bif_sel_a[loop_i] = 3'b100;
        else if(m_work_mode_cfg.e_gpp_type_array[loop_j] == GPP_PCIE_G4X2_0 | m_work_mode_cfg.e_gpp_type_array[loop_j+2] == GPP_PCIE_G4X2_0)
            group_bif_sel_a[loop_i] = 3'b001;
        else if(m_work_mode_cfg.e_gpp_type_array[loop_j+2] == GPP_PCIE_G4X1_0 )
            group_bif_sel_a[loop_i] = 3'b010;
        else if(m_work_mode_cfg.e_gpp_type_array[loop_j] == GPP_PCIE_G4X1_0 )
            group_bif_sel_a[loop_i] = 3'b011;
        else 
            group_bif_sel_a[loop_i] = 3'b000;// should not go this else
    end	
    for(loop_i=0; loop_i<PCIE_GROUP_NUM; loop_i++) begin
	    `uvm_info("map_vip_idx_to_dsp_idx", $sformatf("group_buf_sel[%0d] is %3b ", loop_i, group_bif_sel_a[loop_i]), UVM_LOW)
    end	/*}}}*/


endfunction : dyn_change_work_mode_config /*}}}*/

//------------------------------------------
// set_test_seq  //don't delete
//------------------------------------------
function void test_second_register_after_speed_change_fulleq::set_test_seq();
/*{{{*/

	//uplink sequence
    //uvm_config_db#(uvm_object_wrapper)::set(this, "m_chip_env.m_vsqr.m_pcie_upc_vsqr.main_phase", "default_sequence",pcie_register_after_fulleq_uplink_seq::type_id::get());
	
	foreach(dn_case_link_idx[i])begin
		//uvm_config_db#(uvm_object_wrapper)::set(this, $psprintf("m_chip_env.m_vsqr.m_pcie_vsqr[%0d].main_phase",dn_case_link_idx[i]), "default_sequence",pcie_speed_change_after_fulleq_dnlink_seq::type_id::get());
	end	
	wait_sequence = 1'b1;
endfunction : set_test_seq /*}}}*/
//------------------------------------------
// change_link_cfg
//------------------------------------------
function void test_second_register_after_speed_change_fulleq::change_link_cfg();
/*{{{*/
	//highest_data_rate = $urandom_range(4, 5);
	highest_data_rate = 1;
	//the case flow will happens on the highest data rate
	start_data_rate = highest_data_rate;
	end_data_rate = highest_data_rate;
	eq_mode_used = FULL_EQ;

endfunction : change_link_cfg /*}}}*/

`endif 








