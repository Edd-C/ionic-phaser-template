<template>
    <ion-page>
        <ion-split-pane when="md" content-id="main">
            <ion-menu content-id="main">
                <ion-header>
                    <ion-toolbar color="tertiary">
                        <ion-title>Menu</ion-title>
                    </ion-toolbar>
                </ion-header>
                <ion-content class="ion-padding"> Menu Content </ion-content>
            </ion-menu>

            <div class="ion-page" id="main">
                <ion-header>
                    <ion-toolbar>
                        <ion-title>Main View</ion-title>
                    </ion-toolbar>
                </ion-header>
                <ion-content class="ion-padding">
                    <div id="phaser-example"></div>
                </ion-content>
            </div>
        </ion-split-pane>
    </ion-page>
</template>

<script setup lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonMenu, IonSplitPane } from '@ionic/vue';

import Phaser from 'phaser';
import { onMounted, onUpdated } from 'vue';

function preload() {
    this.load.setBaseURL('http://labs.phaser.io');
    this.load.image('sky', 'assets/skies/space3.png');
    this.load.image('logo', 'assets/sprites/phaser3-logo.png');
    this.load.image('red', 'assets/particles/red.png');
}

function create() {
    this.add.image(400, 300, 'sky');

    const particles = this.add.particles('red');

    const emitter = particles.createEmitter({
        speed: 100,
        scale: { start: 1, end: 0 },
        blendMode: 'ADD',
    });

    const logo = this.physics.add.image(400, 100, 'logo');

    logo.setVelocity(100, 200);
    logo.setBounce(1, 1);
    logo.setCollideWorldBounds(400, 400);
    console.log('this.sys.game.canvas', this.sys.game.canvas);

    emitter.startFollow(logo);
}

function update() {
    //console.log('this.sys.game.canvas', this.sys.game.canvas);
}

const config = {
    type: Phaser.AUTO,
    parent: 'phaser-example',
    physics: {
        default: 'arcade',
        arcade: {
            gravity: { y: 200 },
        },
    },
    height: 400,
    width: 400,
    expandParent: true,
    scene: {
        preload: preload,
        create: create,
        update,
    },
    scale: {
        mode: Phaser.Scale.RESIZE,
        // ...
    },
};

onMounted(() => {
    console.log('Mounted', '');
    new Phaser.Game(config);
    //game.scale.refresh();
    //game.canvas.getContext('2d', { willReadFrequently: true });

    //
});

onUpdated(() => {
    console.log('Updated', '');
});
</script>

<style scoped>
#container {
    text-align: center;

    position: absolute;
    left: 0;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
}

#container strong {
    font-size: 20px;
    line-height: 26px;
}

#container p {
    font-size: 16px;
    line-height: 22px;

    color: #8c8c8c;

    margin: 0;
}

#container a {
    text-decoration: none;
}
</style>
