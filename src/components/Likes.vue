<template>
    <button v-if="!liked" @click="likePost(postId)" class="btn">
        <i class="far fa-thumbs-up likeBtn like"></i>
        {{likes.length}}
    </button>
    <button v-else @click="unlikePost(postId)" class="btn">
        <i class="far fa-thumbs-up likeBtn liked"></i>
        {{likes.length}}
    </button>
</template>

<script>
import axios from 'axios';
export default {
    name: 'Likes',
    props: {
        postId: Number,
        userId: Number
    },
    data() {
        return {
            likes: [],
            liked: null   
        }
    },
    methods: {
        /* axios des Likes en fonction de l'id du post concerné */      
        async axiosLikes(postId) {
            const resLikes = await axios.get(`http://localhost:3000/api/post/${JSON.stringify(postId)}/likes`)
            const dataLikes = await resLikes.json()
            dataLikes.forEach(like => {
            like.userId == this.userId ? this.liked = true : this.like = false // <- ici on vérifie si notre user à déjà liker ce post
        })
            return dataLikes
        },
        /* fonction qui like le post (selon son id) */
        likePost(postId) {
            const data = {
                like: true,
                userId: this.userId,
                postId: postId
            }
            axios.post(`http://localhost:3000/api/post/${JSON.stringify(postId)}/like`, {
                
                headers: {
                    'Content-Type': 'application/json',
                },
                body: data
            })
                .then(res => res.json())
                .then(data => this.likes.push(data))
                .catch(error => console.log(error))
            this.liked = true // <- on indiquer à notre template que le user à liker ce post
        },
        /* fonction pour unliker le post */
        unlikePost(postId) {
            const data = {
                userId: this.userId
            }
            axios.delete(`http://localhost:3000/api/post/${JSON.stringify(postId)}/unlike`, {
                
                headers: {
                    'Content-Type': 'application/json',
                },
                body: data
            })
            this.likes = this.likes.filter((like) => like.userId != this.userId) // <- pour unliker le post côté front
            this.liked = false
        }
    },
    async created() {
        this.likes = await this.axiosLikes(this.postId)
    }
}
</script>

<style scoped>
button {
    background: none;
    border-style: none;
    outline: none;
    width: 40%
}
p {
    margin: 1rem 0 1rem 0;
}
.liked {
    color: white;
    background-color: #3174e4;
    border: 1px solid #3174e4;
    border-radius: 50%;
    padding: 0.5rem;
}
.like {
    color: #3174e4;
    background-color: white;
    border: 1px solid #3174e4;
    border-radius: 50%;
    padding: 0.5rem;
}
i {
    margin-right: 4px;
}
</style>
