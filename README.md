//@version=5
indicator("XAU Sandy ALL IN ONE - PENDEK 2C", overlay=true, max_boxes_count=500, max_labels_count=500)

// === SETTING PENDEK 2C ===
len = input.int(3, "Panjang Pivot")
tunggu = input.int(2, "Tunggu Bar")
pct = input.float(8, "Box % ATR") / 100

atr = ta.atr(85)
ph = ta.pivothigh(high, len, len)
pl = ta.pivotlow(low, len, len)

isSell = not na(ph[tunggu])
isBuy = not na(pl[tunggu])

// === EMA10 & PSAR FILTER ===
ema10 = ta.ema(close, 10)
psar = ta.sar(0.02, 0.02, 0.2)

plot(ema10, "EMA10", color.yellow, 2)
plot(psar, "PSAR", style=plot.style_circles, color=color.lime, linewidth=3)

// === BOX PENDEK 2C ===
sz = atr * pct
if not na(ph)
    box.new(bar_index[len], ph, bar_index[len]+30, ph-sz, bgcolor=color.new(color.red, 70), border_color=color.red, xloc=xloc.bar_index)
if not na(pl)
    box.new(bar_index[len], pl+sz, bar_index[len]+30, pl, bgcolor=color.new(color.green, 70), border_color=color.green, xloc=xloc.bar_index)

// === SIGNAL ===
plotshape(isSell, style=shape.triangledown, location=location.abovebar, color=color.red, text="SELL")
plotshape(isBuy, style=shape.triangleup, location=location.belowbar, color=color.green, text="BUY")

alertcondition(isBuy, "BUY Signal", "PENDEK 2C BUY")
alertcondition(isSell, "SELL Signal", "PENDEK 2C SELL")
