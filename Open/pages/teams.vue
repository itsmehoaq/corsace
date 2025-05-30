<template>
    <div class="teams_list">
        <SubHeader
            :selections="[
                { text: $t('open.teams.teamList'), value: 'list' },
                { text: $t('open.teams.teamManagement'), value: 'management' },
            ]"
            :current-page="page"
            @update:page="page = $event"
        />
        <div 
            v-if="page === 'list'"
            class="teams_list__main_content"
        >
            <OpenTitle>
                {{ $t('open.teams.teamList') }}
                <template #right>
                    <OpenFilter>
                        <template #view>
                            <div
                                :class="{ 'open_filter__selected': showUnregistered }"
                                @click="showUnregistered = true"
                            >
                                {{ $t('open.teams.showAll') }}
                            </div>
                            <div
                                :class="{ 'open_filter__selected': !showUnregistered }"
                                @click="showUnregistered = false"
                            >
                                {{ $t('open.teams.showRegistered') }}
                            </div>
                        </template>
                        <template #sort>
                            <div
                                v-for="sort in sorts"
                                :key="sort"
                                :class="{ 'open_filter__selected': currentSort === sort }"
                                @click="currentSort === sort ? (sortDir = sortDir === 'ASC' ? 'DESC' : 'ASC') : (currentSort = sort)"
                            >
                                <div
                                    v-if="currentSort === sort"
                                    class="open_filter__arrows"
                                >
                                    <div :class="{ 'open_filter__arrows--selected': sortDir === 'ASC' }">
                                        ▲
                                    </div>
                                    <div :class="{ 'open_filter__arrows--selected': sortDir === 'DESC' }">
                                        ▼
                                    </div>
                                </div>
                                {{ $t(`open.components.filter.sorts.${sort}`) }}
                            </div>
                        </template>
                    </OpenFilter>
                    <SearchBar
                        :placeholder="`${$t('open.teams.searchPlaceholder')}`"
                        style="margin-bottom: 10px;"
                        @update:search="searchValue = $event"
                    />
                </template>
            </OpenTitle>
            <div
                v-if="filteredTeams.length !== 0" 
                class="teams_list__main_content_list"
            >
                <OpenCardTeam
                    v-for="team in filteredTeams"
                    :key="team.ID"
                    :team="team"
                    registered
                />
            </div>
            <div
                v-else-if="loading"
                class="teams_list__main_content"
            >
                {{ $t('open.status.loading') }}...
            </div>
            <div
                v-else
                class="teams_list__main_content"
            >
                {{ $t('open.teams.noRegisteredTeams') }}
            </div>
        </div>
        <div 
            v-else-if="page === 'management' && loggedInUser?.discord.userID"
            class="teams_list__main_content"
        >
            <OpenTitle>
                {{ $t('open.teams.teamManagement') }}
                <template #right>
                    <ContentButton
                        class="content_button--red"
                        :link="'team/create'"
                    >
                        {{ $t('open.create.title') }}
                    </ContentButton>
                </template>
            </OpenTitle>
            <div 
                v-if="filteredTeams.length !== 0"
                class="teams_list__main_content_list"
            >
                <OpenCardTeam
                    v-for="team in filteredTeams"
                    :key="team.ID"
                    :team="team"
                    :registered="teamList && teamList.some(t => t.ID === team.ID)"
                />
            </div>
            <div
                v-else
                class="teams_list__main_content"
            >
                {{ $t('open.teams.noCreatedTeams') }}
            </div>
        </div>
        <div
            v-else-if="page === 'management'"
            class="teams_list__main_content"
        >
            {{ $t('open.teams.loginManagement') }}
        </div>
        <div
            v-else-if="loading"
            class="teams_list__main_content"
        >
            {{ $t('open.status.loading') }}...
        </div>
        <div
            v-else 
            class="teams_list__main_content"
        >
            {{ $t('open.teams.error') }}...
        </div>
    </div>
</template>

<script lang="ts">
import { Mixins, Component } from "vue-property-decorator";
import { State, namespace } from "vuex-class";
import { ExtendedPublicationContext } from "centrifuge";
import CentrifugeMixin from "../../Assets/mixins/centrifuge";

import { Tournament } from "../../Interfaces/tournament";
import { Team, TeamList } from "../../Interfaces/team";
import { UserInfo } from "../../Interfaces/user";

import OpenFilter from "../../Assets/components/open/OpenFilter.vue";
import SearchBar from "../../Assets/components/SearchBar.vue";
import OpenTitle from "../../Assets/components/open/OpenTitle.vue";
import ContentButton from "../../Assets/components/open/ContentButton.vue";
import OpenCardTeam from "../../Assets/components/open/OpenCardTeam.vue";
import SubHeader from "../../Assets/components/open/SubHeader.vue";

const openModule = namespace("open");

@Component({
    components: {
        OpenFilter,
        SearchBar,
        OpenTitle,
        ContentButton,
        OpenCardTeam,
        SubHeader,
    },
    head () {
        return {
            title: this.$store.state.open.title,
            meta: [
                {hid: "description", name: "description", content: "Resurrection Cup 2025 (also known as Resurrection Cup: Regulus Revolution), one of osu! standard's largest tournaments. Organized by Phreel and Hoaq, gathering some of the most skilled to the most strategic players from the community. All this to determine who's securing the winner's throne... until next year that is." || ""},

                {hid: "og:site_name", property: "og:site_name", content: "Resurrection Cup 2025"},
                {hid: "og:title", property: "og:title", content: "Resurrection Cup 2025"},
                {hid: "og:url", property: "og:url", content: `https://rescup.xyz${this.$route.path}`}, 
                {hid: "og:description", property: "og:description", content: "Resurrection Cup 2025 (also known as Resurrection Cup: Regulus Revolution), one of osu! standard's largest tournaments. Organized by Phreel and Hoaq, gathering some of the most skilled to the most strategic players from the community. All this to determine who's securing the winner's throne... until next year that is." || ""},
                {hid: "og:image",property: "og:image", content: require("../../Assets/img/site/open/banner.png")},
                
                {name: "twitter:title", content: "Resurrection Cup 2025"},
                {name: "twitter:description", content: "Resurrection Cup 2025 (also known as Resurrection Cup: Regulus Revolution), one of osu! standard's largest tournaments. Organized by Phreel and Hoaq, gathering some of the most skilled to the most strategic players from the community. All this to determine who's securing the winner's throne... until next year that is." || ""},
                {name: "twitter:image", content: require("../../Assets/img/site/open/banner.png")},
                {name: "twitter:image:src", content: require("../../Assets/img/site/open/banner.png")},
            ],
            link: [{rel: "canonical", hid: "canonical", href: `https://rescup.xyz${this.$route.path}`}],
        };
    },
})
export default class Teams extends Mixins(CentrifugeMixin) {

    @State loggedInUser!: null | UserInfo;
    @openModule.State tournament!: Tournament | null;
    @openModule.State myTeams!: Team[] | null;
    @openModule.State teamList!: TeamList[] | null;

    loading = true;
    showUnregistered = false;
    sortDir: "ASC" | "DESC" = "ASC";
    sorts = ["RANK", "BWS AVG", "A-Z", "ID", "TEAM SIZE"] as const;
    sortFunctions: Record<typeof this.sorts[number], (a: TeamList, b: TeamList) => number> = {
        "RANK": (a, b) => a.rank - b.rank,
        "BWS AVG": (a, b) => a.BWS - b.BWS,
        "A-Z": (a, b) => a.name.localeCompare(b.name),
        "ID": (a, b) => a.ID - b.ID,
        "TEAM SIZE": (a, b) => a.members.length - b.members.length,
    };
    currentSort: typeof this.sorts[number] = "BWS AVG";
    searchValue = "";
    page: "list" | "management" = "list";
    unregisteredTeams: TeamList[] | null = null;

    get filteredTeams () {
        if (this.page === "management")
            return this.myTeams ?? [];
        let teams = [...(this.teamList ?? [])];
        if (this.showUnregistered && this.unregisteredTeams)
            teams = [...teams, ...this.unregisteredTeams];
        if (!this.searchValue) // consider asc/desc
            return teams.sort((a, b) => this.sortDir === "ASC" ? this.sortFunctions[this.currentSort](a, b) : this.sortFunctions[this.currentSort](b, a)); 

        return teams.filter(team => 
            team.name.toLowerCase().includes(this.searchValue.toLowerCase()) ||
            team.members.some(member => member.username.toLowerCase().includes(this.searchValue.toLowerCase())) ||
            team.ID.toString().includes(this.searchValue.toLowerCase()) ||
            team.members.some(member => member.osuID.toLowerCase().includes(this.searchValue.toLowerCase()))
        ).sort((a, b) => this.sortFunctions[this.currentSort](a, b) * (this.sortDir === "ASC" ? 1 : -1));
    }

    async mounted () {
        if (this.$route.query.s === "my")
            this.page = "management";
        this.loading = true;
        if (this.tournament) {
            await this.$store.dispatch("open/setTeamList", this.tournament.ID);
            const { data } = await this.$axios.get<{ teams: TeamList[] }>(`/api/tournament/${this.tournament.ID}/unregisteredTeams`);
            if (!data.success) {
                alert("Failed to get unregistered teams, check console for more information");
                console.error(data.error);
                return;
            }
            this.unregisteredTeams = data.teams;
        }
        this.loading = false;

        if (this.tournament)
            await this.initCentrifuge(`teams:${this.tournament.ID}`);
    }

    handleData (ctx: ExtendedPublicationContext) {
        if (ctx.data.type === "teamRegistered")
            this.$store.commit("open/addTeamList", ctx.data.team);
    }
}
</script>

<style lang="scss">
@import '@s-sass/_variables';

.teams_list {

    &__button {
        min-width: 150px;
        text-transform: uppercase;
        padding: 0;
    }

    &__main_content {
        align-self: center;
        position: relative;
        width: 65vw;
        padding: 35px;

        &_list {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            justify-content: flex-start;
            margin-top: 25px;
        }
    }
}
</style>
