<template>
<div class="page-wrap">
    <div class="vend-container">
        <div class="vend-outer">
            <div class="return-box"></div>
            <div class="display-box">
                <div class="display" v-for="(coke, index) in cokes" :key="index">
                    {{ coke.name }} ({{ coke.stock }})
                </div>
            </div>

            <div class="pay-box">{{ amount }} K</div>
            <div class="soda-count">{{ stocks }} Botol</div>
            <div class="coin-box">
                <div class="coin-outter" v-for="(coin, index) in coins" :key="index" @click="addAmount(coin.value)">
                    <p class=" coin">{{ coin.name }}</p>
                </div>
            </div>
            <div class="vend-back">
                <div class="product-row">
                    <div class="soda-box pepsi-box" id="pepsiBox">
                        <div class="soda pepsi" id="pepsi">
                            <p class="soda-text">Aqua</p>
                        </div>
                    </div>
                    <div class="price-box pepsi-price">
                        <p>.5K</p>
                    </div>
                </div>
                <div class="product-row"></div>
                <div class="product-row">
                    <div class="soda-box coke-box" id="cokeBox">
                        <div class="soda coke" id="coke">
                            <p class="soda-text">Teh Botol</p>
                        </div>
                    </div>
                    <div class="price-box coke-price">
                        <p>.7K</p>
                    </div>
                </div>
                <div class="product-row"></div>
                <div class="product-row">
                    <div class="soda-box sprite-box" id="spriteBox">
                        <div class="soda sprite" id="sprite">
                            <p class="soda-text">Pocari Sweat</p>
                        </div>
                    </div>
                    <div class="price-box sprite-price">
                        <p>.10K</p>
                    </div>
                </div>
                <div class="vend-glass"></div>
            </div>
            <div class="touch-pad">
                <div class="button" v-for="(coke, index) in cokes" :key="index" @click="stockreduce(coke.id)">
                    <p>{{ coke.name }}</p>
                </div>
            </div>
            <div class="grab-box">
                <span v-for="(p, index) in purchase" :key="index">
                    {{ p.name }}({{ p.stock }})
                </span>
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: 'VendingMachine',
    data() {
        return {
            stocks: 0,
            amount: 0,
            cokes: [{
                    id: 1,
                    name: 'Aqua',
                    price: 5,
                    stock: 10,
                },
                {
                    id: 2,
                    name: 'Teh Botol',
                    price: 7,
                    stock: 10,
                },
                {
                    id: 3,
                    name: 'Pocari',
                    price: 10,
                    stock: 10,
                },
            ],
            coins: [{
                    id: 1,
                    name: '2K',
                    value: 2,
                },
                {
                    id: 2,
                    name: '5K',
                    value: 5,
                },
                {
                    id: 3,
                    name: '10K',
                    value: 10,
                },
                {
                    id: 4,
                    name: '20K',
                    value: 20,
                },
            ],
            purchase: [],
        };
    },
    computed: {
        totalCoins() {
            return 1;
        },
    },
    watch: {
        /*
            stocks(newStocks, lastStocks) {
                console.log(newStocks, lastStocks);
            },
            cokes(newStocks, lastStocks) {
                console.log(newStocks, lastStocks);
          
            },
        */
    },
    methods: {
        stockleft() {
            const sum = this.cokes.reduce((acc, item) => acc + item.stock, 0);
            this.stocks = sum;
        },
        stockreduce(id) {
            const index = this.cokes.findIndex((e) => e.id === id);
            if (this.cokes[index].stock < 1) {
                alert(` ${this.cokes[index].name} stock habis.`);
            } else {
                if (this.amount >= this.cokes[index].price) {
                    this.cokes[index].stock -= 1;
                    this.stockleft();
                    this.amount -= this.cokes[index].price;
                    this.addPurchase(this.cokes[index]);
                } else {
                    alert(`Uangmu tidak cukup. Tersisa  ${this.amount}K.`);
                }
            }
        },
        addAmount(value) {
            this.amount += value;
        },
        addPurchase(item) {
            const index = this.purchase.findIndex((e) => e.name === item.name);
            if (index < 0) {
                this.purchase.push({
                    name: item.name,
                    stock: 1,
                });
            } else {
                this.purchase[index].stock++;
            }
        },
    },
    mounted() {
        this.stockleft();
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
.page-wrap {
    height: 100vh;
    position: relative;
    padding: 75px 15px 50px 15px;
}

.vend-outer {
    width: 400px;
    box-shadow: 0 0 10px 1px rgba(0, 0, 0, 0.3);
    margin: auto;
    height: 500px;
    background-color: #e7263b;
    padding: 20px;
    position: relative;
    border-radius: 5px;
    z-index: 9999;
}

.display-box {
    position: absolute;
    bottom: 22px;
    left: 15px;
    right: 15px;
    height: 25px;
    padding: 0px 12px;
    display: flex;
    justify-content: space-around;
    align-items: center;
}

.display {
    padding: 0 5px;
    background-color: #333;
    -moz-box-shadow: inset 0 0 10px #000000;
    -webkit-box-shadow: inset 0 0 10px #000000;
    box-shadow: inset 0 0 10px #000000;
    color: goldenrod;
    align-self: Stretch;
    display: flex;
    justify-content: center;
    align-items: center;
}

.pay-box {
    position: absolute;
    top: 113px;
    right: 35px;
    background-color: #333;
    width: 52px;
    height: 25px;
    -moz-box-shadow: inset 0 0 10px #000000;
    -webkit-box-shadow: inset 0 0 10px #000000;
    box-shadow: inset 0 0 10px #000000;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #ddd;
}

.coin-box {
    position: absolute;
    right: 10px;
    top: 150px;
    width: 100px;
    height: 25px;
    display: flex;
    justify-content: space-around;
}

.coin-outter {
    background-color: #ddd;
    width: 25px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #666666;
    box-shadow: 0 0 5px 2px rgba(0, 0, 0, 0.3);
    cursor: pointer;
    margin: 0 2px;
}

.help-box {
    position: absolute;
    top: 0;
    right: 0;
    background-color: #333;
    padding: 10px 20px;
    border-radius: 5px;
    color: white;
}

.coin-outter:active {
    box-shadow: none;
}

.soda-count {
    position: absolute;
    right: 10px;
    top: 19px;
    background-color: #333;
    -moz-box-shadow: inset 0 0 10px #000000;
    -webkit-box-shadow: inset 0 0 10px #000000;
    box-shadow: inset 0 0 10px #000000;
    width: 100px;
    height: 25px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: goldenrod;
    border-radius: 5px;
}

.pepsi-box {
    position: absolute;
    top: 0;
    left: 23px;
    width: 45px;
    height: 100%;
}

.coke-box {
    position: absolute;
    top: 0;
    bottom: 0;
    right: 127px;
}

.sprite-box {
    position: absolute;
    top: 0;
    bottom: 0;
}

.soda,
.grab-soda {
    position: absolute;
    z-index: 999;
    height: 30px;
    border-radius: 5px;
    overflow: hidden;
    box-shadow: 0 0 5px 1px rgba(0, 0, 0, 0.3);
    width: 100px;
}

.return-box {
    position: absolute;
    top: 300px;
    right: 55px;
    background-color: #333;
    width: 10px;
    height: 15px;
    box-shadow: 0 0 10px 1px rgba(0, 0, 0, 0.3);
    cursor: pointer;
}

.return-box:active {
    box-shadow: none;
}

.coke {
    background-color: #d5212b;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    bottom: 0;
    left: 0;
}

.sprite {
    background-color: #17a053;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    bottom: 0;
    left: 0;
}

.pepsi {
    background-color: #004a7e;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    bottom: 0;
    left: 0px;
    transition: all 0.5s;
}

.price-box {
    color: #ddd;
    position: absolute;
    z-index: 99;
}

.coke-price {
    top: 75px;
    left: 210px;
}

.pepsi-price {
    top: 75px;
    left: 60px;
}

.sprite-price {
    top: 75px;
    right: 90px;
}

.touch-pad {
    background-color: #333;
    position: absolute;
    right: 5%;
    top: 35%;
    width: 75px;
    height: 100px;
    display: inline-grid;
    text-align: left;
    -moz-box-shadow: inset 0 0 10px #000000;
    -webkit-box-shadow: inset 0 0 10px #000000;
    box-shadow: inset 0 0 10px #000000;
    border-radius: 5px;
}

.button {
    border: 1px solid #444;
    flex-basis: calc(50% - 2px);
    color: #ddd;
    align-self: stretch;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
}

.button:active {
    background-color: #555;
}

.button p {
    margin: 0;
}

.vend-back {
    background-color: #000;
    position: relative;
    height: 90%;
    width: 75%;
    -moz-box-shadow: inset 0 0 10px #000000;
    -webkit-box-shadow: inset 0 0 10px #000000;
    box-shadow: inset 0 0 10px #000000;
    overflow: hidden;
    border-radius: 5px;
}

.product-row {
    background-color: #111;
    border-bottom: 4px solid black;
    height: 81px;
    -moz-box-shadow: inset 0 0 10px #000000;
    -webkit-box-shadow: inset 0 0 10px #000000;
    box-shadow: inset 0 0 10px #000000;
    display: flex;
    justify-content: space-around;
    align-items: flex-end;
    position: relative;
}

.product-row .tag {
    position: absolute;
    color: #aaa;
}

.product-row .tag-one {
    left: 29px;
    top: 0;
}

.product-row .tag-two {
    left: 178px;
    top: 0;
}

.product-row .tag-three {
    left: 254px;
    top: 0;
}

.product-row .tag-four {
    left: 103px;
    top: 0;
}

.spiral {
    background: transparent;
    border: 2px solid silver;
    border-radius: 100%;
    width: 20px;
    height: 20px;
    box-shadow: 0 0 5px 1px rgba(0, 0, 0, 1);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9;
}

.inner-spiral {
    width: 60%;
    height: 60%;
    border: 2px solid silver;
    border-radius: 100%;
    z-index: 8;
    box-shadow: 0 0 5px 1px rgba(0, 0, 0, 1);
    display: flex;
    justify-content: center;
    align-items: center;
}

.vend-glass {
    background: rgba(255, 255, 255, 0.25);
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 999;
    border-radius: 5px;
}

.grab-box {
    background-color: #333;
    position: absolute;
    right: 2%;
    top: 70.5%;
    width: 100px;
    height: 88px;
    display: grid;
    flex-wrap: wrap;
    -moz-box-shadow: inset 0 0 10px #000000;
    -webkit-box-shadow: inset 0 0 10px #000000;
    box-shadow: inset 0 0 10px #000000;
    border-radius: 5px;
    z-index: 99;
    overflow: hidden;
    color: white;
}

.bottom-box {
    padding-top: 150px;
    background-color: #70d3d0;
    border-top: 1px solid #ddd;
    padding: 25px;
    text-align: center;
    color: #fff;
    z-index: 9999;
}

.heart,
.bottom-box a {
    color: #e7263b;
    text-decoration: none;
}
</style>
