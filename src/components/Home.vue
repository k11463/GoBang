<template>
    <div>
        <div class="box">
            <div class="title">
                五子棋
            </div>
            <div class="gameArea">
                <div class="game">
                    <div class="boxes" @click='move(box)' v-for='box in boxesData' :key='box'>
                        <div class="black" v-if='box.status == 2'>●</div>
                        <div class="white" v-if='box.status == 1'>●</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script> // 至善製作
export default {
    mounted(){
        var idValue = 0;
        for(let i = 0; i < 7; i++){ // 目前7x7 9x9 >>> i = 9, j = 9 // 白棋 = 1 黑棋 = 2
            for(let j = 0; j < 7; j++){
                this.boxesData.push({
                    id: idValue,
                    posX: i,
                    posY: j,
                    status: 0,
                });
                idValue++;
            }
        };
        for (let i = 0; i < 7; i++){ // 7x7橫向贏法
            for (let j = 0; j < 3; j++){
                this.list = [];
                for(let k = 0; k < 5; k++){
                    this.list.push((i * 7) + (j * 1) + k);
                }
                this.winList[0].push(this.list);
            }
        };

        for (let i = 0; i < 7; i++){ // 直向贏法
            for (let j = 0; j < 3; j++){
                this.list = [];
                for (let k = 0; k < 5; k++){
                    this.list.push(i + ((j + k) * 7));
                }
                this.winList[0].push(this.list);
            }
        };

        for (let i = 0; i < 15; i++){ // 左上到右下贏法
            var a = [1, 7], b = [2, 14];
            if (i == 0){
                for (let j = 0; j < 3; j++){
                        this.list = [];
                        for (let k = 0; k < 5; k++){
                            this.list.push(i + ((j + k) * 8));
                        }
                        this.winList[0].push(this.list);
                    }
            };
            a.forEach((el) => {
                if (i == el){
                    for (let j = 0; j < 2; j++){
                        this.list = [];
                        for (let k = 0; k < 5; k++){
                            this.list.push(i + ((j + k) * 8));
                        }
                        this.winList[0].push(this.list);
                    }
                }
            });
            b.forEach((el) => {
                if (i == el){
                    for (let j = 0; j < 1; j++){
                        this.list = [];
                        for (let k = 0; k < 5; k++){
                            this.list.push(i + ((j + k) * 8));
                        }
                        this.winList[0].push(this.list);
                    }
                }
            });
        };

        for (let i = 0; i < 21; i++){ // 右上到左下贏法
            var a = [5, 13], b = [4, 20];
            if (i == 6){
                for (let j = 0; j < 3; j++){
                        this.list = [];
                        for (let k = 0; k < 5; k++){
                            this.list.push(i + ((j + k) * 6));
                        }
                        this.winList[0].push(this.list);
                    }
            };
            a.forEach((el) => {
                if (i == el){
                    for (let j = 0; j < 2; j++){
                        this.list = [];
                        for (let k = 0; k < 5; k++){
                            this.list.push(i + ((j + k) * 6));
                        }
                        this.winList[0].push(this.list);
                    }
                }
            });
            b.forEach((el) => {
                if (i == el){
                    for (let j = 0; j < 1; j++){
                        this.list = [];
                        for (let k = 0; k < 5; k++){
                            this.list.push(i + ((j + k) * 6));
                        }
                        this.winList[0].push(this.list);
                    }
                }
            });
        };
    },
    data(){
        return {
            player: 1,
            boxesData: [],
            list: [],
            winList: [[]], // 贏法列表
            gameOver: false,
            winner: 'none',
        }
    },
    methods: {
        move(val){
            //console.log(this.winList[0].length);
            if (!this.gameOver){
                if (val.status != 0){
                    alert('這邊已經有人下過囉!!');
                }
                else{
                    if (this.player == 1){
                        val.status = 1;
                        this.player = 2;
                    }
                    else {
                        val.status = 2;
                        this.player = 1;
                    }
                }

                for (let i = 0; i < this.winList[0].length; i++){
                    if (this.boxesData[this.winList[0][i][0]].status == 1 && 
                        this.boxesData[this.winList[0][i][1]].status == 1 && 
                        this.boxesData[this.winList[0][i][2]].status == 1 && 
                        this.boxesData[this.winList[0][i][3]].status == 1 && 
                        this.boxesData[this.winList[0][i][4]].status == 1){
                            alert("白棋獲勝!!!");
                            this.gameOver = true;
                    }

                    if (this.boxesData[this.winList[0][i][0]].status == 2 && 
                        this.boxesData[this.winList[0][i][1]].status == 2 && 
                        this.boxesData[this.winList[0][i][2]].status == 2 && 
                        this.boxesData[this.winList[0][i][3]].status == 2 && 
                        this.boxesData[this.winList[0][i][4]].status == 2){
                            alert("黑棋獲勝!!!");
                            this.gameOver = true;
                            this.gameOver = true;
                    }
                }
            }
        }
    },
}
</script>

<style lang="scss" scoped>
*{
    user-select: none;
}
.box{
    width: 100vw;
    height: 100vh;
    .title{
        font-size: 48px;
        font-weight: bold;
        width: 100vw;
        height: 120px;
        display: flex;
        align-items: center;
        justify-content: center;
        padding-top: 80px;
    }
    .gameArea{
        width: 100vw;
        height: 720px;
        .game{
            width: 525px;
            height: 525px;
            margin-left: 50%;
            transform: translateX(-50%);
            margin-top: 80px;
            background-color: #CF9E59;
            .boxes{
                float: left;
                font-size: 100px;
                width: 75px;
                height: 75px;
                background:
                    linear-gradient(to bottom, transparent 49%, #000 48%, #000 52%, transparent 51%),
                    linear-gradient(to right, transparent 49%, #000 48%, #000 52%, transparent 51%);
                display: flex;
                justify-content: center;
                align-items: center;
                transition-duration: .8s;
                .black{
                    color: black;
                }
                .white{
                    color: white;
                }
            }
            .boxes:hover{
                background-color: rgb(187, 143, 81);
            }
        }
    }
}
</style>
