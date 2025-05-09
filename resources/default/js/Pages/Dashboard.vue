<template>
  <app-layout>
    <app-head />

    <hero-section
      v-if="themeSettings.enable_home_hero_section"
      :settings="themeSettings"
    />

    <div class="flex flex-col md:flex-row gap-4 px-2 py-4 mx-auto md:gap-6 md:py-12 md:px-10 lg:px-16 max-w-screen-2xl justify-center">
      <!-- Left Column -->
      <div
        v-if="
          generalSettings.enable_mcserver_onlineplayersbox ||
            generalSettings.enable_voteforserverbox ||
            generalSettings.enable_didyouknowbox ||
            generalSettings.enable_discordbox ||
            generalSettings.enable_donation_box
        "
        class="w-full order-1 md:order-none md:w-1/4 space-y-4 flex-shrink-0"
      >
        <online-players-box v-if="generalSettings.enable_mcserver_onlineplayersbox" />
        <voting-sites-box
          :votingsites="generalSettings.voteforserverbox_content"
          :enabled="generalSettings.enable_voteforserverbox"
        />
        <poll-box :poll="latestPoll" />
        <did-you-know-box :enabled="generalSettings.enable_didyouknowbox" />
        <discord-server-box
          :enabled="generalSettings.enable_discordbox"
          :server="generalSettings.discord_server_id"
        />
        <donation-box />
      </div>

      <!-- Middle Column -->
      <div class="w-full order-3 md:order-none md:w-2/4 space-y-4 flex-shrink-0 flex-grow">
        <VerifyYourEmailBox v-if="$page.props.jetstream.hasEmailVerification && $page.props.auth.user && $page.props.auth.user.email_verified_at === null" />
        <version-check v-if="$page.props.auth.user && isStaff($page.props.auth.user)" />
        <welcome-box
          v-if="generalSettings.enable_welcomebox"
          :html-data="welcomeBoxContentHtml"
        />
        <ingame-chat-box
          :default-server-id="chatDefaultServerId"
          :server-list="chatServerList"
        />
        <top-players-list-box
          :enabled="generalSettings.enable_topplayersbox"
          :players="top10Players"
          :title="__('Top 10 Players')"
        />
        <latest-pinned-news :newslist="pinnedNewsList" />
        <post-list-box v-if="generalSettings.enable_status_feed" />
      </div>

      <!-- Right Column -->
      <div
        v-if="
          generalSettings.enable_mcserver_statuspingbox ||
            generalSettings.enable_shoutbox ||
            newslist.length > 0 ||
            generalSettings.enable_onlineuserbox ||
            generalSettings.enable_newuserbox ||
            generalSettings.enable_socialbox
        "
        class="w-full order-2 md:order-none md:w-1/4 space-y-4 flex-shrink-0"
      >
        <server-status-box />
        <shout-box />
        <news-box :newslist="newslist" />
        <online-users-box
          :users="onlineUsers"
          :enabled="generalSettings.enable_onlineuserbox"
        />
        <newest-user-box
          :user="newestUser"
          :enabled="generalSettings.enable_newuserbox"
        />
        <social-channel-box
          :enabled="generalSettings.enable_socialbox"
          :show-title="true"
          :twitter="generalSettings.twitter_url"
          :twitch="generalSettings.twitch_url"
          :youtube="generalSettings.youtube_url"
          :facebook="generalSettings.facebook_url"
          :linkedin="generalSettings.linkedin_url"
          :tiktok="generalSettings.tiktok_url"
          :discord="generalSettings.discord_invite_url"
          :instagram="generalSettings.instagram_url"
          :whatsapp="generalSettings.whatsapp_url"
          :telegram="generalSettings.telegram_url"
          :reddit="generalSettings.reddit_url"
          :threads="generalSettings.threads_url"
          :github="generalSettings.github_url"
        />
      </div>
    </div>
  </app-layout>
</template>

<script>
// Script section remains unchanged
import AppLayout from '@/Layouts/AppLayout.vue';
import NewsBox from '@/Shared/NewsBox.vue';
import ShoutBox from '@/Shared/ShoutBox.vue';
import DiscordServerBox from '@/Shared/DiscordServerBox.vue';
import NewestUserBox from '@/Shared/NewestUserBox.vue';
import VotingSitesBox from '@/Shared/VotingSitesBox.vue';
import WelcomeBox from '@/Shared/WelcomeBox.vue';
import PostListBox from '@/Shared/PostListBox.vue';
import LatestPinnedNews from '@/Shared/LatestPinnedNews.vue';
import SocialChannelBox from '@/Shared/SocialChannelBox.vue';
import DidYouKnowBox from '@/Shared/DidYouKnowBox.vue';
import VersionCheck from '@/Shared/VersionCheck.vue';
import OnlinePlayersBox from '@/Shared/OnlinePlayersBox.vue';
import IngameChatBox from '@/Shared/IngameChatBox.vue';
import ServerStatusBox from '@/Shared/ServerStatusBox.vue';
import PollBox from '@/Shared/PollBox.vue';
import DonationBox from '@/Shared/DonationBox.vue';
import OnlineUsersBox from '@/Shared/OnlineUsersBox.vue';
import TopPlayersListBox from '@/Shared/TopPlayersListBox.vue';
import VerifyYourEmailBox from '@/Shared/VerifyYourEmailBox.vue';
import HeroSection from '@/Shared/HeroSection.vue';
import {useAuthorizable} from '@/Composables/useAuthorizable';

export default {
    components: {
        HeroSection,
        VerifyYourEmailBox,
        TopPlayersListBox,
        OnlineUsersBox,
        DonationBox,
        PollBox,
        ServerStatusBox,
        IngameChatBox,
        OnlinePlayersBox,
        DidYouKnowBox,
        VersionCheck,
        SocialChannelBox,
        PostListBox,
        WelcomeBox,
        NewsBox,
        AppLayout,
        ShoutBox,
        DiscordServerBox,
        NewestUserBox,
        VotingSitesBox,
        LatestPinnedNews
    },
    props: {
        pinnedNewsList: Array,
        newslist: Array,
        newestUser: Object,
        latestPoll: Object,
        onlineUsers: Array,
        welcomeBoxContentHtml: {
            type: [String,null]
        },
        chatDefaultServerId: Number,
        chatServerList: Array,
        top10Players: Array,
        themeSettings: Object,
    },
    setup() {
        const {canWild, isStaff} = useAuthorizable();
        return {canWild, isStaff};
    },
    data() {
        return {
            generalSettings: this.$page.props.generalSettings
        };
    }
};
</script>