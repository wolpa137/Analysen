# Datenanalyse verschiedener finanzieller Aspekte von Tesla, BMW und VW
import yfinance as yf
import pandas as pd
import matplotlib.pyplot as plt

tesla = yf.Ticker("TSLA")
bmw = yf.Ticker("BMW.DE")
vw = yf.Ticker("VOW3.DE")


data = {"Firma": [tesla.info["longName"], bmw.info["longName"], vw.info["longName"]],
        "KGV": [tesla.info.get("trailingPE"), bmw.info.get("trailingPE"), vw.info.get("trailingPE")],
        "Umsatz": [tesla.info.get("totalRevenue"), bmw.info.get("totalRevenue"), vw.info.get("totalRevenue")]}
 
df = pd.DataFrame(data)

#Statistik 1 - reine Aktien
plt.figure()

tesla_hist = tesla.history(period = "1y")
bmw_hist = bmw.history(period = "1y")
vw_hist = vw.history(period = "1y")

tesla_hist1 = tesla.history(period = "1mo")
bmw_hist1 = bmw.history(period = "1mo")
vw_hist1 = vw.history(period = "1mo")

avg_tesla = tesla_hist["Close"].mean()
avg_bmw = bmw_hist["Close"].mean()
avg_vw = vw_hist["Close"].mean()

plt.plot(tesla_hist["Close"], label = "Tesla", color = "#791515")
plt.plot(bmw_hist["Close"], label = "BMW", color = "#210B67")
plt.plot(vw_hist["Close"], label = "VW", color = "#085234")

plt.axhline(avg_tesla, linestyle="--", label= "Tesla Durchschnitt", color = "#791515")
plt.axhline(avg_bmw, linestyle="--", label = "BMW Durchschnitt", color = "#210B67")
plt.axhline(avg_vw, linestyle = "--", label = "VW Durchschnitt", color = "#085234")

plt.title("Aktienkurse (1 Jahr)")
plt.xlabel("Datum")
plt.ylabel("Preis")

plt.legend()
plt.savefig("charts1.pdf")

#Statistik 2 - normalisierte Aktien
plt.figure()

tesla_norm = tesla_hist["Close"] / tesla_hist["Close"].iloc[0] 
bmw_norm = bmw_hist["Close"] / bmw_hist["Close"].iloc[0] 
vw_norm = vw_hist["Close"] / vw_hist["Close"].iloc[0] 

avg_tesla_norm = tesla_hist["Close"].mean()/tesla_hist["Close"].iloc[0]
avg_bmw_norm = bmw_hist["Close"].mean()/bmw_hist["Close"].iloc[0]
avg_vw_norm = vw_hist["Close"].mean()/vw_hist["Close"].iloc[0]

plt.plot(tesla_norm, label="Tesla", color = "#791515")
plt.plot(bmw_norm, label = "BMW", color = "#210B67")
plt.plot(vw_norm, label = "VW", color = "#085234")

plt.axhline(avg_tesla_norm, linestyle="--", label= "Tesla Durchschnitt", color = "#791515")
plt.axhline(avg_bmw_norm, linestyle="--", label = "BMW Durchschnitt", color = "#210B67")
plt.axhline(avg_vw_norm, linestyle = "--", label = "VW Durchschnitt", color = "#085234")

plt.title("Aktienkurse (1 Jahr) (normalisiert)")
plt.xlabel("Datum")
plt.ylabel("Preis")

plt.legend()
plt.savefig("charts2.pdf")

#Statistik 3 - KGV
plt.figure()

firmen = ["Tesla", "BMW", "VW"]
kgv = [
    tesla.info.get("trailingPE"),
    bmw.info.get("trailingPE"),
    vw.info.get("trailingPE")
]
plt.bar(firmen, kgv)

plt.title("KVG Vergleich")
plt.ylabel("KGV")

for i, value in enumerate(kgv):
    plt.text(i, value, f"{value:.1f}", ha='center', va = 'bottom')

plt.savefig("charts3.pdf")

#Statistik 4
plt.figure()

firmen = ["Tesla", "BMW", "VW"]
umsatz = [tesla.info.get("totalRevenue"), 
          bmw.info.get("totalRevenue"), 
          vw.info.get("totalRevenue")]

plt.bar(firmen, umsatz, color = "#ff000071")

plt.title("Umsatz Vergleich")
plt.ylabel("Umsatz")

for i, value in enumerate(umsatz):
    plt.text(i, value, f"{value:.1f}", ha = "center", va = "bottom")

plt.savefig("charts4.pdf")

#versuch 5
plt.figure()

plt.plot(tesla_hist["Volume"], label="Tesla Volumen", color = "#791515")
plt.plot(bmw_hist["Volume"], label = "BMW Volumen", color = "#210B67")
plt.plot(vw_hist["Volume"], label = "VW Volumen", color = "#085234")

plt.title("Volumen Firmen")
plt.xlabel("Datum")
plt.ylabel("Volumen")

plt.legend()
plt.savefig("charts5.pdf")

#statistik 6 - Preis + Volumen in einem Diagramm
fig, ax1 = plt.subplots()

ax1.plot(tesla_hist["Close"], color = "#791515", label = "Preis Tesla")
ax1.set_xlabel("Datum")
ax1.set_ylabel("Preis", color = "#791515")

ax2 = ax1.twinx()

ax2.bar(tesla_hist.index, tesla_hist["Volume"], label = "Volumen Tesla")

ax2.set_ylabel("Volumen")

plt.title("Preis und Volumen Tesla")

plt.savefig("charts6.pdf")

#Statistik 7 - Preis + Volumen BMW

fig, ax1 = plt.subplots()

ax1.plot(bmw_hist["Close"], label = "BMW Preis", color = "#791515")
ax1.set_ylabel("Preis", color = "#791515")
ax1.set_xlabel("Datum")

ax2 = ax1.twinx()

ax2.bar(bmw_hist.index, bmw_hist["Volume"], label = "BMW Volumen")

ax2.set_ylabel("Volumen")

plt.title("Volumen und Preis BMW")
plt.savefig("charts7.pdf")

#Statistik 8 - Preis und Volumen VW

fig, ax1 = plt.subplots()

ax1.plot(vw_hist["Close"], label = "Preis VW", color = "#791515")
ax1.set_ylabel("Preis")
ax1.set_xlabel("Datum")

ax2 = ax1.twinx()

ax2.bar(vw_hist.index, vw_hist["Volume"], label = "VW Volumen")
ax2.set_ylabel("Volumen")

plt.title("Preis und VOlumen VW 1 Jahr")
plt.savefig("charts8.pdf")

#statistik 9 - Preis und Volumen 1 Woche

fig, axs = plt.subplots(2,2, figsize = (10,8))
plt.subplots_adjust(wspace=0.4)
plt.subplots_adjust(hspace=0.6)

ax = axs[0,0]

ax.bar(tesla_hist1.index, tesla_hist1["Volume"], label = "Tesla Volumen", color = "#9882E1")
ax.set_ylabel("Volumen", color = "#9882E1")
ax.set_title("Preis und Volumen Tesla")

ax1 = ax.twinx()

ax1.plot(tesla_hist1["Close"], label = "Tesla Preis", color = "#791515")
ax1.set_xlabel("Datum")
ax1.set_ylabel("Preis")

ax = axs[0,1]

ax.bar(bmw_hist1.index, bmw_hist1["Volume"], label = "BMW Volumen", color = "#9882E1")
ax.set_ylabel("Volumen", color = "#9882E1")
ax.set_title("Preis und Volumen BMW")

ax1 = ax.twinx()

ax1.plot(bmw_hist1["Close"], label = "BMW Preis", color = "#791515")
ax1.set_xlabel("Datum")
ax1.set_ylabel("Preis")

ax =axs[1,0]

ax.bar(vw_hist1.index, vw_hist1["Volume"], label = "VW Volumen", color = "#9882E1")
ax.set_ylabel("Volumen", color = "#9882E1")
ax.set_title("Preis und Volumen VW")

ax1 = ax.twinx()

ax1.plot(vw_hist1["Close"], label = "BMW Preis", color = "#791515")
ax1.set_xlabel("Datum")
ax1.set_ylabel("Preis")

axs[1,1].axis("off")

fig.suptitle("Volumen und Preis im letzten Monat")

for row in axs:
    for ax in row:
        ax.tick_params(axis='x', rotation=45)

plt.savefig("charts9.pdf")
