## LeeCode每日一题：设计推特

设计一个简化版的推特(Twitter)，可以让用户实现发送推文，关注/取消关注其他用户，能够看见关注人（包括自己）的最近十条推文。你的设计需要支持以下的几个功能：

- **postTweet(userId, tweetId)** : 创建一条新的推文
- **getNewsFeed(userId)** : 检索最近的十条推文。每个推文都必须是由此用户关注的人或者是用户自己发出的。推文必须按照时间顺序由最近的开始排序。
- **follow(followerId, followeeId)** : 关注一个用户
- **unfollow(followerId, followeeId)** : 取消关注一个用户

示例:

```
Twitter twitter = new Twitter();

// 用户1发送了一条新推文 (用户id = 1, 推文id = 5).
twitter.postTweet(1, 5);

// 用户1的获取推文应当返回一个列表，其中包含一个id为5的推文.
twitter.getNewsFeed(1);

// 用户1关注了用户2.
twitter.follow(1, 2);

// 用户2发送了一个新推文 (推文id = 6).
twitter.postTweet(2, 6);

// 用户1的获取推文应当返回一个列表，其中包含两个推文，id分别为 -> [6, 5].
// 推文id6应当在推文id5之前，因为它是在5之后发送的.
twitter.getNewsFeed(1);

// 用户1取消关注了用户2.
twitter.unfollow(1, 2);

// 用户1的获取推文应当返回一个列表，其中包含一个id为5的推文.
// 因为用户1已经不再关注用户2.
twitter.getNewsFeed(1);
```



## Solve

#### 哈希表 + 链表 + 优先队列

记录 **liweiwei1419** 大佬的答案。

链接：[https://leetcode-cn.com/problems/design-twitter/solution/ha-xi-biao-lian-biao-you-xian-dui-lie-java-by-liwe/](https://leetcode-cn.com/problems/design-twitter/solution/ha-xi-biao-lian-biao-you-xian-dui-lie-java-by-liwe/)


```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.PriorityQueue;
import java.util.Set;

public class Twitter {

    /**
     * 用户 id 和推文（单链表）的对应关系
     */
    private Map<Integer, Tweet> twitter;

    /**
     * 用户 id 和他关注的用户列表的对应关系
     */
    private Map<Integer, Set<Integer>> followings;

    /**
     * 全局使用的时间戳字段，用户每发布一条推文之前 + 1
     */
    private static int timestamp = 0;

    /**
     * 合并 k 组推文使用的数据结构（可以在方法里创建使用），声明成全局变量非必需，视个人情况使用
     */
    private static PriorityQueue<Tweet> maxHeap;

    /**
     * Initialize your data structure here.
     */
    public Twitter() {
        followings = new HashMap<>();
        twitter = new HashMap<>();
        maxHeap = new PriorityQueue<>((o1, o2) -> -o1.timestamp + o2.timestamp);
    }

    /**
     * Compose a new tweet.
     */
    public void postTweet(int userId, int tweetId) {
        timestamp++;
        if (twitter.containsKey(userId)) {
            Tweet oldHead = twitter.get(userId);
            Tweet newHead = new Tweet(tweetId, timestamp);
            newHead.next = oldHead;
            twitter.put(userId, newHead);
        } else {
            twitter.put(userId, new Tweet(tweetId, timestamp));
        }
    }

    /**
     * Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
     */
    public List<Integer> getNewsFeed(int userId) {
        // 由于是全局使用的，使用之前需要清空
        maxHeap.clear();

        // 如果自己发了推文也要算上
        if (twitter.containsKey(userId)) {
            maxHeap.offer(twitter.get(userId));
        }

        Set<Integer> followingList = followings.get(userId);
        if (followingList != null && followingList.size() > 0) {
            for (Integer followingId : followingList) {
                Tweet tweet = twitter.get(followingId);
                if (tweet != null) {
                    maxHeap.offer(tweet);
                }
            }
        }

        List<Integer> res = new ArrayList<>(10);
        int count = 0;
        while (!maxHeap.isEmpty() && count < 10) {
            Tweet head = maxHeap.poll();
            res.add(head.id);

            // 这里最好的操作应该是 replace，但是 Java 没有提供
            if (head.next != null) {
                maxHeap.offer(head.next);
            }
            count++;
        }
        return res;
    }


    /**
     * Follower follows a followee. If the operation is invalid, it should be a no-op.
     *
     * @param followerId 发起关注者 id
     * @param followeeId 被关注者 id
     */
    public void follow(int followerId, int followeeId) {
        // 被关注人不能是自己
        if (followeeId == followerId) {
            return;
        }

        // 获取我自己的关注列表
        Set<Integer> followingList = followings.get(followerId);
        if (followingList == null) {
            Set<Integer> init = new HashSet<>();
            init.add(followeeId);
            followings.put(followerId, init);
        } else {
            if (followingList.contains(followeeId)) {
                return;
            }
            followingList.add(followeeId);
        }
    }


    /**
     * Follower unfollows a followee. If the operation is invalid, it should be a no-op.
     *
     * @param followerId 发起取消关注的人的 id
     * @param followeeId 被取消关注的人的 id
     */
    public void unfollow(int followerId, int followeeId) {
        if (followeeId == followerId) {
            return;
        }

        // 获取我自己的关注列表
        Set<Integer> followingList = followings.get(followerId);

        if (followingList == null) {
            return;
        }
        // 这里删除之前无需做判断，因为查找是否存在以后，就可以删除，反正删除之前都要查找
        followingList.remove(followeeId);
    }

    /**
     * 推文类，是一个单链表（结点视角）
     */
    private class Tweet {
        /**
         * 推文 id
         */
        private int id;

        /**
         * 发推文的时间戳
         */
        private int timestamp;
        private Tweet next;

        public Tweet(int id, int timestamp) {
            this.id = id;
            this.timestamp = timestamp;
        }
    }

    public static void main(String[] args) {

        Twitter twitter = new Twitter();
        twitter.postTweet(1, 1);
        List<Integer> res1 = twitter.getNewsFeed(1);
        System.out.println(res1);

        twitter.follow(2, 1);

        List<Integer> res2 = twitter.getNewsFeed(2);
        System.out.println(res2);

        twitter.unfollow(2, 1);

        List<Integer> res3 = twitter.getNewsFeed(2);
        System.out.println(res3);
    }
}
```


面向对象编程


```c++
int global_Time = 0;//发表推文的时间 全局的
//推文类
class Tweet {
public:
    int id;
    int time;
    Tweet *next;
    
    Tweet(int id) {
        this->id = id;
        this->time = global_Time++;
        next = nullptr;
    }
};

//用户类
class User {
public:
    int id;
    Tweet *tweet; //该用户发送的推文 是个链表
    unordered_set<int> follows; //该用户关注的用户

    User(int id) {
        this->id = id;
        tweet = nullptr;
    }

    void follow(int followeeId) {
        //不能关注自己
        if (followeeId == id) return;
        follows.insert(followeeId);
    }

    void unfollow(int followeeId) {
        //没有关注 或者 不能取关自己
        if (!follows.count(followeeId) || followeeId == id) return;
        follows.erase(followeeId);
    }

    void post(int tweetId) {
        Tweet *newTweet = new Tweet(tweetId);
        //链表 采用头插法 新发表插在前面
        newTweet->next = tweet;
        //新的链表
        tweet = newTweet;
    }
};

class Twitter {    
private:
    unordered_map<int, User*> user_map;  //所有的用户
    
    bool contain(int id) {
        return user_map.find(id) != user_map.end();
    }
    
public:
    Twitter() {
        user_map.clear();
    }
    
    void postTweet(int userId, int tweetId) {
        if (!contain(userId)) {
            user_map[userId] = new User(userId);
        }
        //用户 发表 推文  面向对象
        user_map[userId]->post(tweetId);
    }
    
    vector<int> getNewsFeed(int userId) {
        //用户不存在
        if (!contain(userId)) return {};

        struct cmp {
            bool operator()(const Tweet *a, const Tweet *b) {
                return a->time < b->time;
            }
        };
        //构造大顶堆 时间最大的排在最上面
        priority_queue<Tweet*, vector<Tweet*>, cmp> q;

        //自己的推文链表
        if (user_map[userId]->tweet) {
            q.push(user_map[userId]->tweet);
        }
        //关注的推文链表
        for (int followeeId : user_map[userId]->follows) {
            if (!contain(followeeId)) {
                continue;
            }
            Tweet *head = user_map[followeeId]->tweet;
            if (head == nullptr) continue;
            q.push(head);
        }
        vector<int> rs;
        while (!q.empty()) {
            Tweet *t = q.top(); 
            q.pop();
            rs.push_back(t->id);
            if (rs.size() == 10) return rs;
            if (t->next) {
                q.push(t->next);
            }
        }
        return rs;
    }
    
    // 用户followerId 关注 用户followeeId.
    void follow(int followerId, int followeeId) {
        if (!contain(followerId)) {
            user_map[followerId] = new User(followerId);
        }
        //面向对象
        user_map[followerId]->follow(followeeId);
    }
    
    // 用户followerId 取关 用户followeeId.
    void unfollow(int followerId, int followeeId) {
        if (!contain(followerId)) return;
        //面向对象
        user_map[followerId]->unfollow(followeeId);
    }
};
```

