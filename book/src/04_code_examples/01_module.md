# Module

```veryl,playground,editable
// module definition
module ModuleA #(
    param ParamA: u32 = 10,
    const ParamB: u32 = 10, // trailing comma is allowed
) (
    i_clk : input  clock            , // `clock` is a special type for clock
    i_rst : input  reset            , // `reset` is a special type for reset
    i_sel : input  logic            ,
    i_data: input  logic<ParamA> [2], // `[]` means unpacked array in SystemVerilog
    o_data: output logic<ParamA>    , // `<>` means packed array in SystemVerilog
) {
    // const parameter declaration
    //   `param` is not allowed in module
    const ParamC: u32 = 10;

    // variable declaration
    var r_data0: logic<ParamA>;
    var r_data1: logic<ParamA>;
    var r_data2: logic<ParamA>;

    // value binding
    let _w_data2: logic<ParamA> = i_data[0];

    // always_ff statement with reset
    //   `always_ff` can take a mandatory clock and a optional reset
    //   `if_reset` means `if (i_rst)`. This conceals reset porality
    //   `()` of `if` is not required
    //   `=` in `always_ff` is non-blocking assignment
    always_ff (i_clk, i_rst) {
        if_reset {
            r_data0 = 0;
        } else if i_sel {
            r_data0 = i_data[0];
        } else {
            r_data0 = i_data[1];
        }
    }

    // always_ff statement without reset
    always_ff (i_clk) {
        r_data1 = r_data0;
    }

    // clock and reset can be omitted
    // if there is a single clock and reset in the module
    always_ff {
        r_data2 = r_data1;
    }

    assign o_data = r_data1;
}
```
