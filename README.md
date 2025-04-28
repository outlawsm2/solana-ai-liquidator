// Target: NetMind AI ($NMT) collateral liquidations  
fn nmt_liquidation(ctx: Context<NmtLiquidate>) -> Result<()> {  
    let nmt_price = get_pyth_price("NMT/USD");  
    let collateral_ratio = compute_ratio(ctx.accounts.position);  

    if collateral_ratio < 1.0 {  
        let liq_tx = build_liq_tx(  
            victim: ctx.accounts.position,  
            oracle: nmt_price,  
            dex: Kamino, // Kamino has the deepest NMT liquidity  
        );  
        submit_blitz_tx(liq_tx)?; // Uses TIP + private RPC  
    }  
    Ok(())  
}  
