# ANALISEPETROBRAS

#DADOS PARA ANALISE FINANCEIRA

#www.quantmod.com

#carregar pacotes
library(quantmod)
library(xts)
library(moments)

#Seleção do periodo de analise

startDate = as.Date('2019-01-21')
endDate = as.Date('2019-11-09')

# Vamos fazer o download do periodo

?getSymbols

getSymbols('PETR4.SA', src = 'yahoo', from = startDate, to  = endDate)

# CHECANDO O TIPO DE DADOS RETORNADO 

class(PETR4.SA)
is.xts(PETR4.SA)

# MOSTRA OS PRIMEIROS REQUISITOS PARA AS AÇÕES DA PETROBRAS
head(PETR4.SA)

# ANALISANDO OS DADOS DO FECHAMENTO 

PETR4.SA.Close <- PETR4.SA[,'PETR4.SA.Close']
is.xts(PETR4.SA.Close)
?Cl

head(Cl(PETR4.SA),5)


# AGORA VAMOS PLOTAR O GRAFICO DA PETROBRAS
# VOU TULIZAR  O GRAFICO CANDLESTICK DA PETROBRAS

?candleChart
candleChart(PETR4.SA)


# PLOT DE FECHAMENTO 

plot(PETR4.SA.Close, main = 'Fechamento diario da Petrobras', col = 'red', xlab = 'Data', ylab = 'Preço',
     major.ticks = 'months', minor.ticks = 'FALSE'
     )

addBBands(n = 20 , sd = 2)

#adicionar o indicador ADX, media 11 tipo exponencial
addADX(n = 11, maType = 'EMA')

#CALCULANDO LOGS DIARIOS

PETR4.SA.ret <- diff(log(PETR4.SA.Close), lag = 1)

#Remover valores NA 

PETR4.SA.ret <- PETR4.SA.ret[-1]


#AGORA VAMOS PLOTAR A TAXA DE RETORNO

plot(PETR4.SA.ret, main = 'Fechamento diario das ações da Petrobras', col= 'red', xlab = 'data',
     ylab= 'Retorno', major.ticks = 'months', minor.ticks = FALSE
     )

#CALCULO MEDIDAS ESTATISTICA

statNames <- c('Mean', 'Standart Deviation', 'Skewness', 'Kurtosis')
PETR4.SA.stats <- c(mean(PETR4.SA.ret), sd(PETR4.SA.ret), skewness(PETR4.SA.ret), kurtosis(PETR4.SA.ret))
names(PETR4.SA.stats) <- statNames
PETR4.SA.stats


# SALVANDO OS DADOS EM UM ARQUIVO .rds (aquivo em formato binario do R)

getSymbols('PETR4.SA', src= 'yahoo')
saveRDS(PETR4.SA, file = "PETR4.SA.rds")# salvar os dados em formato binario
ptr = readRDS('PETR4.SA.rds')
dir()
head(ptr)

