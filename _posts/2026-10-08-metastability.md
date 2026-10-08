---
layout: post
title: "Crossing clock domains without lying to yourself"
summary: "A two-flop synchronizer is not a fix. It is a bet on probability, and the odds are written in the datasheet."
tag: Note
---

This is a sample entry. Delete `_posts/2026-10-08-metastability.md` once you
have seen how a log renders — it exists to show the formatting, not because it
is finished writing.

Every asynchronous signal entering a clocked domain is a chance for a flip-flop
to be caught mid-decision. The output settles eventually. "Eventually" is the
problem.

## The failure nobody simulates

Setup and hold are not suggestions. Violate them and the flop enters a
metastable state, sitting between logic levels for an unbounded time before
resolving one way or the other. Simulation will not show you this. The
testbench resolves instantly and cleanly, and the hardware does not.

The usual answer is a two-stage synchronizer:

```verilog
module sync2 (
    input  wire clk,
    input  wire rst_n,
    input  wire async_in,
    output wire sync_out
);
    (* ASYNC_REG = "TRUE" *) reg [1:0] stage;

    always @(posedge clk or negedge rst_n) begin
        if (!rst_n) stage <= 2'b00;
        else        stage <= {stage[0], async_in};
    end

    assign sync_out = stage[1];
endmodule
```

The `ASYNC_REG` attribute matters. Without it the tools may place the two flops
far apart, and the second one loses most of the settling time the first one
bought you.

## What the numbers actually say

Mean time between failures falls out of the resolution time constant and the
settling window you allow:

| Term | Meaning | Typical |
|---|---|---|
| `tau` | Resolution time constant | 20–50 ps |
| `T_w` | Metastability window | ~10 ps |
| `f_clk` | Destination clock | 100 MHz |
| `f_data` | Transition rate of the input | 1 MHz |

> Two flops at 100 MHz with a 50 ps time constant buys an MTBF measured in
> years. Drop to a single flop and it drops to hours. The circuit looks the
> same on a schematic.

## Where it still bites

A synchronizer protects one bit. It does nothing for a multi-bit bus, where
each line resolves independently and you can latch a value that never existed
on the source side. For a bus you need a handshake, a gray-coded counter, or a
proper asynchronous FIFO.

- Single control bit: two-flop synchronizer
- Multi-bit value, slow updates: handshake with a ready/ack pair
- Multi-bit value, streaming: async FIFO with gray-coded pointers

The failure mode of getting this wrong is a board that passes every test for
three weeks and then hangs once, in the field, with no log entry.
