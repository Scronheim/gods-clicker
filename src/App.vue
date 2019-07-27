<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-lg-9">
                <span v-for="monster of currentMonster" :key="monster.name">
                    <div class="background">
                        <div class="row">
                            <div class="col-lg-4 offset-lg-4">
                                <h1 class="text-center monster-name">{{ monster.name }}</h1>
                                <img :src="monster.img" alt="cerberus" class="monster" @click="clickOnMonster"/>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-lg-4 offset-lg-4">
                                <div class="progress" style="width: 100%; margin-top: 100px">
                                    <div class="progress-bar bg-danger" v-bind:style="'width: '+monster.currentHp+'%'" role="progressbar"  aria-valuenow="25" aria-valuemin="0" aria-valuemax="100">{{ monster.currentHp }}</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </span>
            </div>
            <div class="col-lg-3">
                <div class="row">
                    <img width="70px" alt="faith" src="img/faith_w.png"><p class="faith">{{ faith }}</p>
                    <img width="70px" alt="glory" src="img/glory_w.png" style="margin-left: 4vh"><p class="glory">{{ glory }}</p>
                </div>
                <div class="row">
                    <button type="button" style="background: url('img/aid.jpg'); width: 500px; height: 100px"></button>
                    <button type="button" style="background: url('img/artemida.jpg'); width: 500px; height: 100px"></button>
                    <button type="button" style="background: url('img/brahma.jpg'); width: 500px; height: 100px"></button>
                    <button type="button" style="background: url('img/kali.jpg'); width: 500px; height: 100px"></button>
                    <button type="button" style="background: url('img/odin.jpg'); width: 500px; height: 100px"></button>
                    <button type="button" style="background: url('img/osiris.jpg'); width: 500px; height: 100px"></button>
                    <button type="button" style="background: url('img/ra.jpg'); width: 500px; height: 100px"></button>
                    <button type="button" style="background: url('img/shiva.jpg'); width: 500px; height: 100px"></button>
                    <button type="button" style="background: url('img/skadi.jpg'); width: 500px; height: 100px"></button>
                    <button type="button" style="background: url('img/veles.jpg'); width: 500px; height: 100px"></button>
                    <button type="button" style="background: url('img/zeus.jpg'); width: 500px; height: 100px"></button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                faith: 0,
                glory: 0,
                gods: {
                    aid: {
                        damage: 1,
                        cost: 1
                    },
                    artemida: {
                        damage: 2,
                        cost: 2
                    },
                    brahma: {
                        damage: 3,
                        cost: 3
                    },
                    kali: {
                        damage: 4,
                        cost: 4
                    },
                    odin: {
                        damage: 5,
                        cost: 5
                    }
                },
                monsters: [{
                    cerberus: {
                        name: 'Цербер',
                        hp: 100,
                        currentHp: 100,
                        reward: 10,
                        img: 'img/cerberus.png'
                    }
                },{
                    babaYaga: {
                        name: 'Баба Яга',
                        hp: 200,
                        currentHp: 200,
                        reward: 20,
                        img: 'img/babayaga.png'
                    }
                }],
                currentMonster: {
                    monster: {
                        name: 'Цербер',
                        hp: 100,
                        currentHp: 100,
                        reward: 10,
                        img: 'img/cerberus.png'
                    }
                },
                clickDamage: 50
            }
        },
        watch: {
            'currentMonster.monster.currentHp': function (value) {
                // eslint-disable-next-line
                if (value <= 0) {
                    this.faith += this.currentMonster.monster.reward;
                    this.changeMonster();
                }
            }
        },
        methods: {
            clickOnMonster() {
                if (this.currentMonster.monster.currentHp >= 0) {
                    this.currentMonster.monster.currentHp -= this.clickDamage;
                }
            },
            changeMonster() {
                for (let i of this.monsters) {
                    for (let j in i) {
                        if (i[j].name === this.currentMonster.monster.name) {
                            continue;
                        } else {
                            Object.assign(this.currentMonster.monster, i[j]);
                        }
                    }
                }
                // eslint-disable-next-line
                // console.log(this.currentMonster);
            }
        }
    }
</script>

<style>

</style>
