<template>
    <div class="layout layout--open">
        <DevBanner
            v-if="devBanner"
            @close="hideBanner()"
        />
        <the-header
            class="header"
            :notif="teamInvites && teamInvites.length > 0"
        >
            <a href="/">
                <img
                    src="../../Assets/img/site/open/navlogo.svg"
                    class="header__logo"
                    :class="`header__logo--${viewTheme}`"
                >
            </a>

            <div class="header__nav">
                <NuxtLink
                    to="/"
                    class="header__nav-item"
                >
                    {{ $t("open.navbar.home") }}
                </NuxtLink>
                <NuxtLink
                    to="/info"
                    class="header__nav-item"
                >
                    {{ $t("open.navbar.info") }}
                </NuxtLink>
                <NuxtLink
                    to="/qualifiers"
                    class="header__nav-item"
                >
                    {{ $t("open.navbar.qualifiers") }}
                </NuxtLink>
                <NuxtLink
                    to="/teams"
                    class="header__nav-item"
                >
                    {{ $t("open.navbar.teams") }}
                </NuxtLink>
                <NuxtLink
                    to="/schedule"
                    class="header__nav-item"
                >
                    {{ $t("open.navbar.schedule") }}
                </NuxtLink>

                <NuxtLink
                    to="/mappool"
                    class="header__nav-item"
                >
                    {{ $t("open.navbar.mappool") }}
                </NuxtLink>
                <NuxtLink
                    to="/staff"
                    class="header__nav-item"
                >
                    {{ $t("open.navbar.staff") }}
                </NuxtLink>
            </div>
            <template #menu>
                <NuxtLink
                    v-if="staffInfo?.userRoles.includes(0) || staffInfo?.userRoles.includes(6)"
                    to="/referee"
                >
                    <MenuItem>{{ $t("open.navbar.referee").toString().toLowerCase() }}</MenuItem>
                </NuxtLink>
                <NuxtLink
                    to="/teams?s=my"
                >
                    <MenuItem>{{ $t("open.navbar.myTeams") }}</MenuItem>
                </NuxtLink>
                <NuxtLink
                    to="/team/invites"
                >
                    <MenuItem>
                        {{ $t("open.navbar.invitations") }}
                        <div
                            v-if="teamInvites && teamInvites.length > 0"
                            class="header__notification"
                        />
                    </MenuItem>
                </NuxtLink>
            </template>
        </the-header>

        <nuxt
            class="main"
            :class="`main--${viewTheme}`"
        />

        <the-footer class="footer">
            <div class="socials">
                <Tooltip>
                    <template #icon>
                        <a
                            class="socials__link"
                            href="https://twitter.com/ResurrectionCup"
                            target="_blank"
                        >
                            <img
                                class="socials__icon"
                                :class="`socials__icon--${viewTheme}`"
                                src="../../Assets/img/social/x.svg"
                                alt=""
                            >
                        </a>
                    </template>
                    {{ $t("open.footer.twitter") }}
                </Tooltip>
                <Tooltip>
                    <template #icon>
                        <a
                            class="socials__link"
                            href="https://bsky.app/profile/rescup.bsky.social"
                            target="_blank"
                        >
                            <img
                                class="socials__icon"
                                :class="`socials__icon--${viewTheme}`"
                                src="../../Assets/img/social/bluesky.svg"
                                alt=""
                            >
                        </a>
                    </template>
                    {{ $t("open.footer.blueSky") }}
                </Tooltip>
                <Tooltip>
                    <template #icon>
                        <a
                            class="socials__link"
                            href="https://discord.gg/UNzyfgGfeu"
                            target="_blank"
                        >
                            <img
                                class="socials__icon"
                                :class="`socials__icon--${viewTheme}`"
                                src="../../Assets/img/social/discord.svg"
                                alt=""
                            >
                        </a>
                    </template>
                    {{ $t("open.footer.discord") }}
                </Tooltip>
                <Tooltip>
                    <template #icon>
                        <a
                            class="socials__link"
                            href="https://www.twitch.tv/resurrectioncup"
                            target="_blank"
                        >
                            <img
                                class="socials__icon"
                                :class="`socials__icon--${viewTheme}`"
                                src="../../Assets/img/social/twitch.svg"
                                alt=""
                            >
                        </a>
                    </template>
                    {{ $t("open.footer.twitch") }}
                </Tooltip>
                <Tooltip>
                    <template #icon>
                        <a
                            class="socials__link"
                            href="https://youtube.com/@resurrectioncup"
                            target="_blank"
                        >
                            <img
                                class="socials__icon"
                                :class="`socials__icon--${viewTheme}`"
                                src="../../Assets/img/social/youtube.svg"
                                alt=""
                            >
                        </a>
                    </template>
                    {{ $t("open.footer.youtube") }}
                </Tooltip>
                <Tooltip>
                    <template #icon>
                        <a
                            class="socials__link"
                            href="https://osu.ppy.sh/community/forums/topics/2076778?n=1"
                            target="_blank"
                        >
                            <img
                                class="socials__icon"
                                :class="`socials__icon--${viewTheme}`"
                                src="../../Assets/img/social/osu.svg"
                                alt=""
                            >
                        </a>
                    </template>
                    {{ $t("open.footer.forumPost") }}
                </Tooltip> 
                <Tooltip>
                    <template #icon>
                        <a
                            class="socials__link"
                            href="https://challonge.com/RESC25"
                            target="_blank"
                        >
                            <img
                                class="socials__icon"
                                :class="`socials__icon--${viewTheme}`"
                                src="../../Assets/img/social/challonge.svg"
                                alt=""
                            >
                        </a>
                    </template>
                    {{ $t("open.footer.challonge") }}
                </Tooltip>
                <Tooltip>
                    <template #icon>
                        <a
                            class="socials__link"
                            href="https://docs.google.com/spreadsheets/d/1Fzf9nzjPS8FeX4bR7Kyzj1qnpUB5xbXz0KwH-b7-5ic/edit?rm=minimal"
                        >
                            <img
                                class="socials__icon"
                                :class="`socials__icon--${viewTheme}`"
                                src="../../Assets/img/social/googlesheets.svg"
                                alt=""
                            >
                        </a>
                    </template>
                    {{ $t("open.footer.sheet") }}
                </Tooltip>
                <Tooltip>
                    <template #icon>
                        <a
                            class="socials__link"
                            href="https://ko-fi.com/rescup"
                            target="_blank"
                        >
                            <img
                                class="socials__icon"
                                :class="`socials__icon--${viewTheme}`"
                                src="../../Assets/img/social/kofi.svg"
                                alt=""
                            >
                        </a>
                    </template>
                    {{ $t("open.footer.kofi") }}
                </Tooltip>
            </div>
        </the-footer>
    </div>
</template>

<script lang="ts">
import { Mixins, Component } from "vue-property-decorator";
import { State, namespace } from "vuex-class";
import { ExtendedPublicationContext } from "centrifuge";
import CentrifugeMixin from "../../Assets/mixins/centrifuge";

import { UserInfo } from "../../Interfaces/user";
import { BaseTeam } from "../../Interfaces/team";
import { OpenStaffInfo } from "../../Interfaces/staff";

import DevBanner from "../../Assets/components/DevBanner.vue";
import TheHeader from "../../Assets/components/header/TheHeader.vue";
import TheFooter from "../../Assets/components/footer/TheFooter.vue";
import MenuItem from "../../Assets/components/header/MenuItem.vue";
import Tooltip from "../../Assets/components/Tooltip.vue";

const openModule = namespace("open");

@Component({
    components: {
        DevBanner,
        TheHeader,
        TheFooter,
        MenuItem,
        Tooltip,
    },
    middleware: "index",
})
export default class Default extends Mixins(CentrifugeMixin) {

    @State viewTheme!: "light" | "dark";
    @State loggedInUser!: null | UserInfo;

    @openModule.State teamInvites!: BaseTeam[] | null;
    @openModule.State staffInfo!: OpenStaffInfo | null;

    devBanner = false;
    isSmall = false;

    async mounted() {
        if (process.client) {
            this.isSmall = window.innerWidth < 576;
            window.addEventListener("resize", () => {
                this.isSmall = window.innerWidth < 576;
            });
        }

        await this.$store.dispatch("setViewTheme", "dark");

        // Get devBanner from localStorage, will be a number equivalent to Date.now() if it exists, if it doesn't exist, or if it's been past the value, then show the banner
        const devBannerTime = localStorage.getItem("devBanner");
        if (!devBannerTime || parseInt(devBannerTime) < Date.now())
            this.devBanner = true;

        if (!this.loggedInUser)
            return;

        await this.initCentrifuge(`invitations:${this.loggedInUser.ID}`);
    }

    handleData(ctx: ExtendedPublicationContext) {
        if (ctx.data.type === "invite")
            this.$store.commit("open/addInvite", ctx.data.team);
    }

    hideBanner() {
        localStorage.setItem("devBanner", (Date.now() + 604800000).toString());
        this.devBanner = false;
    }
}
</script>

<style lang="scss">
@import '@s-sass/_mixins';
@import '@s-sass/_variables';

.header {
    border-bottom: 1px solid $open-red;
    background-color: #ffffff;
    background-repeat: no-repeat;
    background-position: left center;
    width: 100vw;
    position: relative;

    &__notification {
        width: 8px;
        height: 8px;
        border-radius: 100%;
        background-color: $open-red;
    }

    &__logo {
        padding-left: 6px;
        @include breakpoint(tablet) {
            padding-left: 7px;
        }
        @include breakpoint(laptop) {
            padding-left: 9px;
        }
        @include breakpoint(desktop) {
            padding-left: 2rem;
        }
        
        height: 100%;
        padding-top: 0.75rem;
        padding-bottom: 0.75rem;
    }

    &__nav {
        position: relative;
        left: 50%;
        transform: translateX(-85%);
        align-self: center;
        display: flex;
        width: 40vw;
        justify-content: space-between;
        align-items: center;

        &-item {
            font-weight: 600;
            text-decoration: none;
            color: $open-red;

            &:hover {
                color: $open-red;
                text-decoration: none;
            }

            &.nuxt-link-exact-active {
                color: $open-red;
                position: relative;
                display: inline-block;
            }

            &.nuxt-link-exact-active::after {
                content: "";
                position: absolute;
                left: calc(50% - 4.5px / 2);
                bottom: -7px;
                width: 4.5px;
                height: 4.5px;
                transform: rotate(-45deg);
                background-color: $open-red;
            }
        }
    }
}

.socials {
    height: 100%;
    display: flex;
    align-items: center;
    gap: 16px;
    @include breakpoint(mobile) {
        margin-right: auto;
    }
    @include breakpoint(laptop) {
        margin-left: 20px;
    }

    &__link {
        display: flex;
    }

    &__icon {
        margin-right: 3px;
        height: 20px;
        filter: brightness(100);
        @include breakpoint(tablet) {
            height: 25px;
        }
        @include breakpoint(laptop) {
            height: 24px;
        }
        // &--light {
        //     filter: invert(1);
        // }
        // &--dark {
        //     color: white;
        // }
    }
}

.main {
    background-size: cover;
    overflow-x: hidden;
}
</style>
