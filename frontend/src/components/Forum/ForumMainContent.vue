<template>
  <div class="main-content space-y-4">
    <div class="create-post bg-white rounded-xl shadow-sm p-4 flex items-center">
      <div class="user-avatar w-10 h-10 rounded-full bg-blue-100 flex items-center justify-center text-white font-bold">
        <span>U</span>
      </div>
      <input type="text"
        class="post-input flex-1 mx-4 px-4 py-2 border border-gray-200 rounded-full text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
        placeholder="What's on your mind?">
      <button
        class="create-post-btn px-4 py-2 bg-blue-500 text-white text-sm font-medium rounded-full hover:bg-blue-600 transition-colors">
        Post
      </button>
    </div>

    <div v-for="post in posts" :key="post.id"
      class="post bg-white rounded-xl shadow-sm p-5 cursor-pointer hover:shadow-md transition-shadow"
      @click="openPost(post)">
      <div class="post-content">
        <h3 class="post-title text-lg font-semibold text-gray-800 mb-3 leading-snug">{{ post.title }}</h3>
        <div class="post-meta flex items-center">
          <div
            class="user-avatar small w-8 h-8 rounded-full bg-orange-100 flex items-center justify-center text-xs text-white font-bold">
            <span>{{ post.author.charAt(0) }}</span>
          </div>
          <div class="meta-details flex-1 ml-3">
            <div class="user-info">
              <span class="username text-sm font-semibold">{{ post.author }}</span>
              <span class="post-time p-2 text-xs text-gray-500 mt-0.5">{{ post.time }}</span>
            </div>
            <div class="post-stats flex gap-4 sm:flex-row flex-col sm:gap-4 gap-1 text-xs text-gray-500">
              <span>{{ post.views }} views</span>
              <span>{{ post.likes }} likes</span>
              <span>{{ post.comments }} comments</span>
            </div>
          </div>
          <button
            class="post-actions text-gray-400 hover:text-gray-600 p-1 rounded hover:bg-gray-100 transition-colors">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M12 5v.01M12 12v.01M12 19v.01M12 6a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2zm0 7a1 1 0 110-2 1 1 0 010 2z">
              </path>
            </svg>
          </button>
        </div>
      </div>
    </div>
    <PostDetailModal v-if="selectedPost" :show="showModal" :post="selectedPost" :currentUser="currentUser"
      @close="closeModal" @add-comment="handleNewComment" />
  </div>
</template>

<script>
import PostDetailModal from './PostDetailModal.vue'

export default {
  name: 'ForumMainContent',
  components: {
    PostDetailModal
  },
  props: {
    posts: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      currentUser: 'johnsmith',
      selectedPost: null,
      showModal: false,
      // Add this comments data structure
      postComments: {
        1: [
          {
            id: 101,
            author: 'MalayLanguageExpert',
            time: '2 days ago',
            content: '"Lah" is a particle used to make sentences more casual and friendly. It often comes at the end of sentences.',
            likes: 24
          },
          {
            id: 102,
            author: 'LanguageLearner123',
            time: '1 week ago',
            content: 'I struggled with this too! My teacher said to use it when you want to soften a statement.',
            likes: 15
          }
        ],
        2: [
          {
            id: 201,
            author: 'PolyglotPro',
            time: '2 days ago',
            content: 'Immersion is key! Try watching Malay movies with subtitles.',
            likes: 38
          }
        ],
        3: [
          {
            id: 301,
            author: 'LinguisticsStudent',
            time: '5 days ago',
            content: 'Yes! There are many dialects. Malaysian Malay differs from Indonesian Malay.',
            likes: 56
          }
        ],
        4: [
          {
            id: 401,
            author: 'HappyUser88',
            time: '1 week ago',
            content: 'I agree! This app helped me learn basic Malay in just 2 months.',
            likes: 18
          }
        ]
      }
    }
  },
  methods: {
    openPost(post) {
      this.selectedPost = {
        ...post,
        content: this.getPostContent(post.id),
        comments: this.postComments[post.id] || [] // Pass comments for this post
      }
      this.showModal = true
    },
    closeModal() {
      this.showModal = false
      this.selectedPost = null
    },
    getPostContent(postId) {
      const contents = {
        1: "I've been learning Malay for a few months now, but I'm really confused about when to use 'lah' in sentences...",
        2: "I'm just starting to learn Malay and feeling a bit overwhelmed. What methods have worked best for you all?...",
        3: "I recently met someone from Indonesia who said they speak Malay, but it sounded quite different from what I've learned...",
        4: "Just wanted to share how much I love this app! I've tried many language learning apps but this one really stands out..."
      }
      return contents[postId] || "No content available"
    },
    handleNewComment(newComment) {
      if (!this.selectedPost) return
      const postId = this.selectedPost.id
      if (!this.postComments[postId]) {
        this.postComments[postId] = []
      }
      this.postComments[postId].unshift(newComment)
      // Update the selected post to trigger reactivity
      this.selectedPost = {
        ...this.selectedPost,
        comments: [...this.postComments[postId]]
      }
    }
  }
}
</script>