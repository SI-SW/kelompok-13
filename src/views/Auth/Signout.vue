<script>
import {mapActions} from 'pinia';
import d$auth from '@/stores/auth';

export default {
  name: "signout",
  methods: {
  ...mapActions(d$auth, ['a$logout']),
  logout() {
    try {
      this.a$logout();
      this.$router.replace({ name: 'Signin' });
    } catch (e) {
      console.log(e);
    }
  },
  }
};

</script>