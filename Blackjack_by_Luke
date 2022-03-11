import random


class Card:

    def __init__(self, rank, suit):
        self.rank = rank
        self.suit = suit

    def getRank(self):
        return self.rank

    def getSuit(self):
        return self.suit


class Deck:
    ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A']
    suits = ['\u2660', '\u2661', '\u2662', '\u2663']

    def __init__(self):
        self.deck = []
        for suit in Deck.suits:
            for rank in Deck.ranks:
                self.deck.append(Card(rank, suit))

    def deal(self, playerhand):
        self.card = self.deck.pop()
        playerhand.append(self.card)
        return self.card

    def shuffle(self, numDeck):

        self.deck = []
        for suit in Deck.suits:
            for rank in Deck.ranks:
                self.deck.append(rank + ' ' + suit)

        self.deck = self.deck * numDeck
        print("Here come the cards!")
        print(self.deck)

        random.shuffle(self.deck)
        print("\nHere comes the shuffle!")
        print(self.deck)

        random.shuffle(self.deck)
        print("\nHere comes another shuffle without showing you the cards!")

        return self.deck


class Player:
    def __init__(self, name, status=0, bet=10, bankroll=1000):
        self.name = name
        self.hand = []
        self.status = status  # 0 = Busted, 1 = Stay, 2 = Double
        self.bet = bet
        self.bankroll = bankroll


def total(hand):
    values = {'2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9, '1': 10,
              'J': 10, 'Q': 10, 'K': 10, 'A': 11}
    result = 0
    numAces = 0

    for card in hand:
        result += values[card[0]]
        if card[0] == 'A':
            numAces += 1

    while result > 21 and numAces > 0:
        result -= 10
        numAces -= 1

    return result


numPlayer = ''
while numPlayer not in [str(x) for x in range(1, 9)]:
    numPlayer = input("How many players (1-8)? ")
numPlayer = int(numPlayer)

player = []
for i in range(numPlayer):
    playername = input("Player " + str(i + 1) + " Name: ")
    newplayer = Player(str(playername))
    player.append(newplayer)

numDeck = 0
while numDeck not in ['1', '2', '3', '4', '5', '6']:
    numDeck = input("How many decks (1-6)? ")
numDeck = int(numDeck)

deck = Deck()
deck.shuffle(numDeck)
dealerhand = []
numRound = 1

dealerbankroll = 1000 * numPlayer
print("\nDealers' Bankroll is ", dealerbankroll, ". Player's Bankroll is 1000.", sep='')

while True:

    print('\nRound', numRound)

    dealerhand = []
    for i in range(numPlayer):
        player[i].hand = []
        wager = ''
        while wager not in [str(x) for x in range(10, int(player[i].bankroll) + 1)]:
            wager = input(player[i].name+', please place bet. Mimumum 10. Bankroll= ' + str(player[i].bankroll) + ' ')
        player[i].bet = int(wager)

    for i in range(2):
        for j in range(numPlayer):
            deck.deal(player[j].hand)
        deck.deal(dealerhand)

    print("\nDealer: ", dealerhand[0])
    for i in range(numPlayer):
        print(player[i].name, ':', player[i].hand, '=', total(player[i].hand))
    print()

    for i in range(numPlayer):

        player[i].status = 1

        answer = 'answer'
        while answer not in {'0', 's', 'S', 'stand', 'Stand', 'STAND', '1', 'h', 'H', 'hit', 'Hit', 'HIT',
                             '2', 'd', 'D', 'double', 'Double', 'DOUBLE'}:
            answer = input(player[i].name+' : '+str(player[i].hand)+' = '+str(total(player[i].hand))+'  0-Stand  1-Hit  2-Double? ')

        while answer in {'1', 'h', 'H', 'hit', 'Hit', 'HIT'}:
            playerhand = []
            deck.deal(playerhand)
            player[i].hand += playerhand
            if total(player[i].hand) == 21 and len(player[i].hand) == 2:
                print(player[i].name, 'had Blackjack!', player[i].hand, '=', total(player[i].hand))
                player[i].status = 1.5
            else:
                print(player[i].name, ':', player[i].hand, '=', total(player[i].hand))

            if total(player[i].hand) > 21:
                player[i].status = 0
                player[i].bankroll -= player[i].bet
                dealerbankroll += player[i].bet
                print(player[i].name, ' busted. Bankroll is $', player[i].bankroll, '.', sep='')
                break

            answer = input(player[i].name+' : '+str(player[i].hand)+' = '+str(total(player[i].hand))+'  0-Stand  1-Hit? ')

        if answer in {'2', 'd', 'D', 'double', 'Double', 'DOUBLE'}:
            playerhand = []
            player[i].status = 2
            deck.deal(playerhand)
            player[i].hand += playerhand
            print(player[i].name, ' :', player[i].hand, '=', total(player[i].hand))

        print()

    print("Dealer : ", dealerhand, total(dealerhand))
    while total(dealerhand) < 17:
        deck.deal(dealerhand)
        print("Dealer : ", dealerhand, '=', total(dealerhand))

        if total(dealerhand) > 21:
            for i in range(numPlayer):
                if player[i].status != 0:
                    print('Dealer busted!\n')
                    player[i].bankroll += player[i].bet * player[i].status
                    dealerbankroll -= player[i].bet * player[i].status
                    print(player[i].name, '\'s bankroll is ', player[i].bankroll, '.', sep="")
            break

    for i in range(numPlayer):
        dealerTotal, playerTotal = total(dealerhand), total(player[i].hand)

        if dealerTotal > 21:
            break
        if player[i].status == 0:
            break

        if dealerTotal == 21 and 2 == len(dealerhand) < len(player[i].hand):
            player[i].bankroll -= player[i].bet * player[i].status
            dealerbankroll += player[i].bet * player[i].status
            print("Dealer had Blackjack", " > ", player[i].name, playerTotal)
            print(player[i].name, "'s bankroll is ", player[i].bankroll, sep="")

        elif playerTotal == 21 and 2 == len(player[i].hand) < len(dealerhand):
            player[i].bankroll += player[i].bet * player[i].status
            dealerbankroll -= player[i].bet * player[i].status
            print(player[i].name, "had Blackjack!", " > ", "Dealer", dealerTotal)
            print(player[i].name, "'s bankroll is ", player[i].bankroll, sep="")

        elif playerTotal == 21 and 2 == len(player[i].hand) == len(dealerhand):
            print(player[i].name, "and", "Dealer both had Blackjack. Push!")
            print(player[i].name, "'s bankroll is ", player[i].bankroll, sep="")

        elif dealerTotal > playerTotal:
            player[i].bankroll -= player[i].bet * player[i].status
            dealerbankroll += player[i].bet * player[i].status
            print('Dealer ', dealerTotal, " > ", player[i].name, ' ', playerTotal, ". Dealer Won!", sep='')
            print(player[i].name, "'s bankroll is ", player[i].bankroll, sep="")

        elif dealerTotal < playerTotal:
            player[i].bankroll += player[i].bet * player[i].status
            dealerbankroll -= player[i].bet * player[i].status
            print("Dealer ", dealerTotal, " < ", player[i].name, " ", playerTotal, ". ", player[i].name, " Won!",
                  sep='')
            print(player[i].name, "'s bankroll is ", player[i].bankroll, sep="")

        else:
            print("Dealer", dealerTotal, "=", player[i].name, playerTotal, "Push!")
            print(player[i].name, "'s bankroll is ", player[i].bankroll, sep="")

        if player[i].bankroll == 0:
            print(player[i].name, "is bankrupted.")

    totalbankroll = 0
    for i in range(numPlayer):
        totalbankroll += player[i].bankroll
    if totalbankroll == 0:
        print("All players are bankrupted. Dealer Won!")
        break

    print("Dealer's Bankroll is", dealerbankroll)

    if dealerbankroll <= 0:
        print("Dealer is bankrupted. Game Over!")
        break

    numRound += 1
