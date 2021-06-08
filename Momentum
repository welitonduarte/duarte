from funcoes import get_ohlc
import MetaTrader5 as mt5
import pandas as pd
import warnings
from matplotlib import pyplot as plt
from dateutil import relativedelta

warnings.filterwarnings("ignore")
mt5.initialize()

df2 = pd.DataFrame()
df3 = pd.DataFrame()
final = pd.DataFrame()

qnt_acoes = 10
qnt_meses = 100

# Ibovespa
ativos =  ['ABEV3', 'AZUL4', 'B3SA3', 'BBAS3', 'BBDC3', 'BBDC4', 'BBSE3', 'BEEF3',
           'BPAC11', 'BRAP4', 'BRDT3', 'BRFS3', 'BRKM5', 'BRML3', 'BTOW3', 'CCRO3', 'CIEL3',
           'CMIG4', 'COGN3', 'CPFE3', 'CPLE6', 'CRFB3', 'CSAN3', 'CSNA3', 'CVCB3', 'CYRE3',
           'ECOR3', 'EGIE3', 'ELET3', 'ELET6', 'EMBR3', 'ENBR3', 'ENEV3', 'ENGI11', 'EQTL3',
           'EZTC3', 'FLRY3', 'GGBR4', 'GNDI3', 'GOAU4', 'GOLL4', 'HAPV3', 'HGTX3', 'HYPE3',
           'IGTA3', 'IRBR3', 'ITSA4', 'ITUB4', 'JBSS3', 'JHSF3', 'KLBN11', 'LAME4', 'LCAM3',
           'LREN3', 'MGLU3', 'MRFG3', 'MRVE3', 'MULT3', 'NTCO3', 'PCAR3', 'PETR3', 'PETR4',
           'PRIO3', 'QUAL3', 'RADL3', 'RAIL3', 'RENT3', 'SANB11', 'SBSP3', 'SULA11', 'SUZB3',
           'TAEE11', 'TIMS3', 'TOTS3', 'UGPA3', 'USIM5', 'VALE3', 'VIVT3', 'VVAR3', 'WEGE3', 'YDUQ3', 'BOVA11']

'''
# SMLL
ativos = ['AALR3', 'ABCB4', 'AERI3', 'AESB3', 'AGRO3', 'ALSO3', 'ALUP11', 'AMAR3', 'AMBP3', 'ANIM3',
          'ARZZ3', 'AZUL4', 'BEEF3', 'BKBR3', 'BMGB4', 'BPAN4', 'BRML3', 'BRPR3', 'BRSR6', 'CAML3', 'CASH3',
          'CEAB3', 'CESP6', 'CIEL3', 'COGN3', 'CSMG3', 'CVCB3', 'CYRE3', 'DIRR3', 'DTEX3', 'ECOR3', 'EMBR3',
          'ENAT3', 'ENBR3', 'ENJU3', 'EVEN3', 'EZTC3', 'FESA4', 'FLRY3', 'GFSA3', 'GOAU4', 'GOLL4', 'GRND3',
          'GUAR3', 'HBOR3', 'HBSA3', 'HGTX3', 'IGTA3', 'IRBR3', 'JHSF3', 'JPSA3', 'LAVV3', 'LCAM3', 'LEVE3',
          'LIGT3', 'LINX3', 'LJQQ3', 'LOGG3', 'LOGN3', 'LPSB3', 'LWSA3', 'MDIA3', 'MEAL3', 'MILS3', 'MOVI3',
          'MRFG3', 'MRVE3', 'MTRE3', 'MULT3', 'MYPK3', 'NGRD3', 'ODPV3', 'OMGE3', 'PARD3', 'PCAR3', 'PETZ3',
          'PNVL3', 'POMO4', 'POSI3', 'PRIO3', 'PTBL3', 'QUAL3', 'RANI3', 'RAPT4', 'RCSL4', 'ROMI3', 'RRRP3',
          'SAPR11', 'SAPR4', 'SBFG3', 'SEER3', 'SEQL3', 'SIMH3', 'SLCE3', 'SMTO3', 'SOMA3', 'SQIA3', 'STBP3',
          'TAEE11', 'TASA4', 'TCSA3', 'TEND3', 'TGMA3', 'TRIS3', 'TUPY3', 'UNIP6', 'VIVA3', 'VLID3', 'VULC3', 'WIZS3', 'YDUQ3', 'SMAL11']
'''

#ativos = ['TASA4']

for ativo in ativos:
    df1 = pd.DataFrame(mt5.copy_rates_from_pos(
        ativo, mt5.TIMEFRAME_MN1, 0, qnt_meses))
    #df1 = pd.DataFrame(mt5.copy_rates_from_pos(ativo, mt5.TIMEFRAME_W1, 0, 80))

    df1 = df1.rename(columns={"open": "Open", 'high': 'High',
                              'low': 'Low', 'close': 'Close', 'volume': 'Volume'})

    loop = True
    while loop == True:
        df1.time = pd.to_datetime(df1['time'], unit='s', errors='ignore')
        if len(df1) >= 1:
            loop = False

    df1 = df1.set_index("time")
    df1.drop(["tick_volume", 'spread', 'real_volume'], axis=1, inplace=True)

    df1['Return' + '_' + ativo] = round(df1['Close'].pct_change(1)*100, 2)
    df1 = df1.dropna()

    df2[ativo] = df1['Return' + '_' + ativo]
    # print(ativo)

all = df2
all = all.dropna(axis=1)

# print(all)
all.to_excel('Modelos\Outros\Momentum//Momentum.xlsx')

while len(all) > 6:
    # print(all)

    df2 = all.head(6)
    df2.loc['Soma'] = df2.sum(axis=0)
    # print(df2)
    resultado_df = df2.sort_values(by='Soma', axis=1)
    #print(resultado_df)

    escolha_long = [resultado_df.index[len(
        resultado_df)-2], resultado_df[resultado_df.columns[-qnt_acoes:]].columns]

    # Ativos Short
    escolha_short = [resultado_df.index[len(
        resultado_df)-2], resultado_df[resultado_df.columns[:qnt_acoes]].columns]

    # print(escolha_long)
    next_month = escolha_long[0] + relativedelta.relativedelta(months=1)

    # Semanal
    #next_month = escolha_long[0] + relativedelta.relativedelta(weeks=1)

    retorno = (all.loc[next_month, escolha_long[1]] / 10).round(2)
    #print(retorno)
    # print(next_month)
    final = final.append(
        {'Time': next_month, 'Return': retorno.sum()/qnt_acoes}, ignore_index=True)
    #final = final.set_index("Time")

    all = all.iloc[1:]
    #print(final)


# print(all)
final = final.set_index("Time")
final.to_excel('Modelos\Outros\Momentum//final.xlsx')

acumulado = ((final['Return']/100+1).cumprod()-1)*100
acumulado.to_excel('Modelos\Outros\Momentum//sum.xlsx')

print(final)
print(acumulado)

plt.plot(acumulado)
plt.show()


'''
plt.bar(resultado_df.columns, resultado_df.loc['Soma'], width = 0.6)
plt.show()
'''
