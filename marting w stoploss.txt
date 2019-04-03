const MartinBet = 2  // bet to martingale
const MartingCashout = 11.00  // martingale cashout
const lostStreak = 8  // how many 1xs before martingaling
const maxLost = 2000   // max number of bits it'll lose
 
let currentBet = 1
let cashOut = 1.01
let lostcount = 0
let losses = 0
let startBR = balance/100
let curBR = balance/100
 
while (true) {
 
    console.log('Placing bet of', Math.round(currentBet),'bits')
    const { multiplier, balance} = await this.bet(Math.round(currentBet)*100, cashOut)
    curBR = balance/100
    if(multiplier < MartingCashout){
        if(currentBet == 1){
            lostcount++
            if(lostcount == lostStreak){
                currentBet = MartinBet
                cashOut = MartingCashout
            }  
        }else{
            currentBet *= 2
        }
        losses = startBR-curBR
        if(losses > maxLost){
            startBR = curBR
            losses = 0
            currentBet = 1
            console.log('Max losses reached.')
            console.log('Resetting everything.')
        }
        console.log('Current losses =', losses);
    }else{
        currentBet = 1
        cashOut = 1.01
        lostcount = 0
        losses = 0
        startBR = curBR
    }
}
