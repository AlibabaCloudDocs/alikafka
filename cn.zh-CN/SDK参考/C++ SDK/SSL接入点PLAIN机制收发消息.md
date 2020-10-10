---
keyword: [kafka, C++, 公网, 收发消息, ssl]
---

# SSL接入点PLAIN机制收发消息

本文介绍如何在公网环境下使用C++ SDK接入消息队列Kafka版的SSL接入点并使用PLAIN机制收发消息。

您已安装GCC。详情请参见[安装GCC](https://gcc.gnu.org/install/)。

## 安装C++依赖库

1.  执行以下命令切换到yum源配置目录/etc/yum.repos.d/。

    ```
    cd /etc/yum.repos.d/
    ```

2.  创建yum源配置文件confluent.repo。

    ```
    [Confluent.dist]
    name=Confluent repository (dist)
    baseurl=https://packages.confluent.io/rpm/5.1/7
    gpgcheck=1
    gpgkey=https://packages.confluent.io/rpm/5.1/archive.key
    enabled=1
    
    [Confluent]
    name=Confluent repository
    baseurl=https://packages.confluent.io/rpm/5.1
    gpgcheck=1
    gpgkey=https://packages.confluent.io/rpm/5.1/archive.key
    enabled=1
    ```

3.  执行以下命令安装C++依赖库。

    ```
    yum install librdkafka-devel
    ```


## 准备配置

1.  [下载SSL根证书文件](https://code.aliyun.com/alikafka/aliware-kafka-demos/blob/master/kafka-cpp-demo/vpc-ssl/ca-cert.pem)。

2.  执行以下命令安装SSL依赖库。

    ```
    yum install openssl openssl-devel
    ```

3.  执行以下命令安装SASL依赖库。

    ```
    yum install cyrus-sasl{,-plain}
    ```


## 发送消息

1.  创建发送消息程序kafka\_producer.c。

    ```
    /*
     * librdkafka - Apache Kafka C library
     *
     * Copyright (c) 2017, Magnus Edenhill
     * All rights reserved.
     *
     * Redistribution and use in source and binary forms, with or without
     * modification, are permitted provided that the following conditions are met:
     *
     * 1. Redistributions of source code must retain the above copyright notice,
     *    this list of conditions and the following disclaimer.
     * 2. Redistributions in binary form must reproduce the above copyright notice,
     *    this list of conditions and the following disclaimer in the documentation
     *    and/or other materials provided with the distribution.
     *
     * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
     * AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
     * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
     * ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE
     * LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
     * CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
     * SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
     * INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
     * CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
     * ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
     * POSSIBILITY OF SUCH DAMAGE.
     */
    
    /**
     * Simple Apache Kafka producer
     * using the Kafka driver from librdkafka
     * (https://github.com/edenhill/librdkafka)
     */
    
    #include <stdio.h>
    #include <signal.h>
    #include <string.h>
    
    
    /* Typical include path would be <librdkafka/rdkafka.h>, but this program
     * is builtin from within the librdkafka source tree and thus differs. */
    #include "librdkafka/rdkafka.h"
    
    
    static int run = 1;
    
    /**
     * @brief Signal termination of program
     */
    static void stop (int sig) {
            run = 0;
            fclose(stdin); /* abort fgets() */
    }
    
    
    /**
     * @brief Message delivery report callback.
     *
     * This callback is called exactly once per message, indicating if
     * the message was succesfully delivered
     * (rkmessage->err == RD_KAFKA_RESP_ERR_NO_ERROR) or permanently
     * failed delivery (rkmessage->err != RD_KAFKA_RESP_ERR_NO_ERROR).
     *
     * The callback is triggered from rd_kafka_poll() and executes on
     * the application's thread.
     */
    static void dr_msg_cb (rd_kafka_t *rk,
                           const rd_kafka_message_t *rkmessage, void *opaque) {
            if (rkmessage->err)
                    fprintf(stderr, "%% Message delivery failed: %s\n",
                            rd_kafka_err2str(rkmessage->err));
            else
                    fprintf(stderr,
                            "%% Message delivered (%zd bytes, "
                            "partition %"PRId32")\n",
                            rkmessage->len, rkmessage->partition);
    
            /* The rkmessage is destroyed automatically by librdkafka */
    }
    
    
    
    int main (int argc, char **argv) {
            rd_kafka_t *rk;         /* Producer instance handle */
            rd_kafka_topic_t *rkt;  /* Topic object */
            rd_kafka_conf_t *conf;  /* Temporary configuration object */
            char errstr[512];       /* librdkafka API error reporting buffer */
            char buf[512];          /* Message value temporary buffer */
            const char *brokers;    /* Argument: broker list */
            const char *topic;      /* Argument: topic to produce to */
            const char *username;      /* Argument: topic to produce to */
            const char *password;      /* Argument: topic to produce to */
    
            /*
             * Argument validation
             */
            if (argc != 5) {
                    fprintf(stderr, "%% Usage: %s <broker> <topic> <username> <password>\n", argv[0]);
                    return 1;
            }
    
            brokers = argv[1];
            topic   = argv[2];
            username = argv[3];
            password = argv[4];
    
    
    
    
            /*
             * Create Kafka client configuration place-holder
             */
            conf = rd_kafka_conf_new();
    
            /* Set bootstrap broker(s) as a comma-separated list of
             * host or host:port (default port 9092).
             * librdkafka will use the bootstrap brokers to acquire the full
             * set of brokers from the cluster. */
            if (rd_kafka_conf_set(conf, "bootstrap.servers", brokers,
                                  errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK) {
                    fprintf(stderr, "%s\n", errstr);
                    return 1;
            }
    
            if (rd_kafka_conf_set(conf, "ssl.ca.location", "ca-cert.pem", errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK
                        || rd_kafka_conf_set(conf, "security.protocol", "sasl_ssl", errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK
                        || rd_kafka_conf_set(conf, "sasl.mechanism", "PLAIN", errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK
                        || rd_kafka_conf_set(conf, "sasl.username", username, errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK
                        || rd_kafka_conf_set(conf, "sasl.password", password, errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK
                        ) {
                    fprintf(stderr, "%s\n", errstr);
                    return -1;
            }
    
            /* Set the delivery report callback.
             * This callback will be called once per message to inform
             * the application if delivery succeeded or failed.
             * See dr_msg_cb() above. */
            rd_kafka_conf_set_dr_msg_cb(conf, dr_msg_cb);
    
    
            /*
             * Create producer instance.
             *
             * NOTE: rd_kafka_new() takes ownership of the conf object
             *       and the application must not reference it again after
             *       this call.
             */
            rk = rd_kafka_new(RD_KAFKA_PRODUCER, conf, errstr, sizeof(errstr));
            if (!rk) {
                    fprintf(stderr,
                            "%% Failed to create new producer: %s\n", errstr);
                    return 1;
            }
    
    
            /* Create topic object that will be reused for each message
             * produced.
             *
             * Both the producer instance (rd_kafka_t) and topic objects (topic_t)
             * are long-lived objects that should be reused as much as possible.
             */
            rkt = rd_kafka_topic_new(rk, topic, NULL);
            if (!rkt) {
                    fprintf(stderr, "%% Failed to create topic object: %s\n",
                            rd_kafka_err2str(rd_kafka_last_error()));
                    rd_kafka_destroy(rk);
                    return 1;
            }
    
            /* Signal handler for clean shutdown */
            signal(SIGINT, stop);
    
            fprintf(stderr,
                    "%% Type some text and hit enter to produce message\n"
                    "%% Or just hit enter to only serve delivery reports\n"
                    "%% Press Ctrl-C or Ctrl-D to exit\n");
    
            while (run && fgets(buf, sizeof(buf), stdin)) {
                    size_t len = strlen(buf);
    
                    if (buf[len-1] == '\n') /* Remove newline */
                            buf[--len] = '\0';
    
                    if (len == 0) {
                            /* Empty line: only serve delivery reports */
                            rd_kafka_poll(rk, 0/*non-blocking */);
                            continue;
                    }
    
                    /*
                     * Send/Produce message.
                     * This is an asynchronous call, on success it will only
                     * enqueue the message on the internal producer queue.
                     * The actual delivery attempts to the broker are handled
                     * by background threads.
                     * The previously registered delivery report callback
                     * (dr_msg_cb) is used to signal back to the application
                     * when the message has been delivered (or failed).
                     */
            retry:
                    if (rd_kafka_produce(
                                /* Topic object */
                                rkt,
                                /* Use builtin partitioner to select partition*/
                                RD_KAFKA_PARTITION_UA,
                                /* Make a copy of the payload. */
                                RD_KAFKA_MSG_F_COPY,
                                /* Message payload (value) and length */
                                buf, len,
                                /* Optional key and its length */
                                NULL, 0,
                                /* Message opaque, provided in
                                 * delivery report callback as
                                 * msg_opaque. */
                                NULL) == -1) {
                            /**
                             * Failed to *enqueue* message for producing.
                             */
                            fprintf(stderr,
                                    "%% Failed to produce to topic %s: %s\n",
                                    rd_kafka_topic_name(rkt),
                                    rd_kafka_err2str(rd_kafka_last_error()));
    
                            /* Poll to handle delivery reports */
                            if (rd_kafka_last_error() ==
                                RD_KAFKA_RESP_ERR__QUEUE_FULL) {
                                    /* If the internal queue is full, wait for
                                     * messages to be delivered and then retry.
                                     * The internal queue represents both
                                     * messages to be sent and messages that have
                                     * been sent or failed, awaiting their
                                     * delivery report callback to be called.
                                     *
                                     * The internal queue is limited by the
                                     * configuration property
                                     * queue.buffering.max.messages */
                                    rd_kafka_poll(rk, 1000/*block for max 1000ms*/);
                                    goto retry;
                            }
                    } else {
                            fprintf(stderr, "%% Enqueued message (%zd bytes) "
                                    "for topic %s\n",
                                    len, rd_kafka_topic_name(rkt));
                    }
    
    
                    /* A producer application should continually serve
                     * the delivery report queue by calling rd_kafka_poll()
                     * at frequent intervals.
                     * Either put the poll call in your main loop, or in a
                     * dedicated thread, or call it after every
                     * rd_kafka_produce() call.
                     * Just make sure that rd_kafka_poll() is still called
                     * during periods where you are not producing any messages
                     * to make sure previously produced messages have their
                     * delivery report callback served (and any other callbacks
                     * you register). */
                    rd_kafka_poll(rk, 0/*non-blocking*/);
            }
    
    
            /* Wait for final messages to be delivered or fail.
             * rd_kafka_flush() is an abstraction over rd_kafka_poll() which
             * waits for all messages to be delivered. */
            fprintf(stderr, "%% Flushing final messages..\n");
            rd_kafka_flush(rk, 10*1000 /* wait for max 10 seconds */);
    
            /* Destroy topic object */
            rd_kafka_topic_destroy(rkt);
    
            /* Destroy the producer instance */
            rd_kafka_destroy(rk);
    
            return 0;
    }
    ```

2.  执行以下命令编译kafka\_producer.c。

    ```
    gcc -lrdkafka ./kafka_producer.c -o kafka_producer
    ```

3.  执行以下命令发送消息。

    ```
    ./kafka_producer <bootstrap_servers> <topic> <username> <password>
    ```

    |参数|描述|
    |--|--|
    |bootstrap\_servers|SSL接入点。您可在消息队列Kafka版控制台的**实例详情**页面的**基本信息**区域获取。|
    |topic|Topic名称。您可在消息队列Kafka版控制台的**Topic管理**页面获取。|
    |username|用户名。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的用户名。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。详情请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。 |
    |password|密码。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的密码。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。详情请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。 |


## 订阅消息

1.  创建订阅消息程序kafka\_consumer.c。

    ```
    /*
     * librdkafka - Apache Kafka C library
     *
     * Copyright (c) 2015, Magnus Edenhill
     * All rights reserved.
     *
     * Redistribution and use in source and binary forms, with or without
     * modification, are permitted provided that the following conditions are met:
     *
     * 1. Redistributions of source code must retain the above copyright notice,
     *    this list of conditions and the following disclaimer.
     * 2. Redistributions in binary form must reproduce the above copyright notice,
     *    this list of conditions and the following disclaimer in the documentation
     *    and/or other materials provided with the distribution.
     *
     * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
     * AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
     * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
     * ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE
     * LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
     * CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
     * SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
     * INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
     * CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
     * ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
     * POSSIBILITY OF SUCH DAMAGE.
     */
    
    /**
     * Apache Kafka high level consumer example program
     * using the Kafka driver from librdkafka
     * (https://github.com/edenhill/librdkafka)
     */
    
    #include <ctype.h>
    #include <signal.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
    #include <syslog.h>
    #include <sys/time.h>
    #include <errno.h>
    #include <getopt.h>
    
    /* Typical include path would be <librdkafka/rdkafka.h>, but this program
     * is builtin from within the librdkafka source tree and thus differs. */
    #include "librdkafka/rdkafka.h"  /* for Kafka driver */
    
    
    static int run = 1;
    static rd_kafka_t *rk;
    static int exit_eof = 0;
    static int wait_eof = 0;  /* number of partitions awaiting EOF */
    static int quiet = 0;
    static     enum {
        OUTPUT_HEXDUMP,
        OUTPUT_RAW,
    } output = OUTPUT_HEXDUMP;
    
    static void stop (int sig) {
            if (!run)
                    exit(1);
        run = 0;
        fclose(stdin); /* abort fgets() */
    }
    
    
    static void hexdump (FILE *fp, const char *name, const void *ptr, size_t len) {
        const char *p = (const char *)ptr;
        unsigned int of = 0;
    
    
        if (name)
            fprintf(fp, "%s hexdump (%zd bytes):\n", name, len);
    
        for (of = 0 ; of < len ; of += 16) {
            char hexen[16*3+1];
            char charen[16+1];
            int hof = 0;
    
            int cof = 0;
            int i;
    
            for (i = of ; i < (int)of + 16 && i < (int)len ; i++) {
                hof += sprintf(hexen+hof, "%02x ", p[i] & 0xff);
                cof += sprintf(charen+cof, "%c",
                           isprint((int)p[i]) ? p[i] : '.');
            }
            fprintf(fp, "%08x: %-48s %-16s\n",
                of, hexen, charen);
        }
    }
    
    /**
     * Kafka logger callback (optional)
     */
    static void logger (const rd_kafka_t *rk, int level,
                const char *fac, const char *buf) {
        struct timeval tv;
        gettimeofday(&tv, NULL);
        fprintf(stdout, "%u.%03u RDKAFKA-%i-%s: %s: %s\n",
            (int)tv.tv_sec, (int)(tv.tv_usec / 1000),
            level, fac, rd_kafka_name(rk), buf);
    }
    
    
    
    /**
     * Handle and print a consumed message.
     * Internally crafted messages are also used to propagate state from
     * librdkafka to the application. The application needs to check
     * the `rkmessage->err` field for this purpose.
     */
    static void msg_consume (rd_kafka_message_t *rkmessage) {
        if (rkmessage->err) {
            if (rkmessage->err == RD_KAFKA_RESP_ERR__PARTITION_EOF) {
                fprintf(stderr,
                    "%% Consumer reached end of %s [%"PRId32"] "
                       "message queue at offset %"PRId64"\n",
                       rd_kafka_topic_name(rkmessage->rkt),
                       rkmessage->partition, rkmessage->offset);
    
                if (exit_eof && --wait_eof == 0) {
                                    fprintf(stderr,
                                            "%% All partition(s) reached EOF: "
                                            "exiting\n");
                    run = 0;
                            }
    
                return;
            }
    
                    if (rkmessage->rkt)
                            fprintf(stderr, "%% Consume error for "
                                    "topic \"%s\" [%"PRId32"] "
                                    "offset %"PRId64": %s\n",
                                    rd_kafka_topic_name(rkmessage->rkt),
                                    rkmessage->partition,
                                    rkmessage->offset,
                                    rd_kafka_message_errstr(rkmessage));
                    else
                            fprintf(stderr, "%% Consumer error: %s: %s\n",
                                    rd_kafka_err2str(rkmessage->err),
                                    rd_kafka_message_errstr(rkmessage));
    
                    if (rkmessage->err == RD_KAFKA_RESP_ERR__UNKNOWN_PARTITION ||
                        rkmessage->err == RD_KAFKA_RESP_ERR__UNKNOWN_TOPIC)
                            run = 0;
            return;
        }
    
        if (!quiet)
            fprintf(stdout, "%% Message (topic %s [%"PRId32"], "
                            "offset %"PRId64", %zd bytes):\n",
                            rd_kafka_topic_name(rkmessage->rkt),
                            rkmessage->partition,
                rkmessage->offset, rkmessage->len);
    
        if (rkmessage->key_len) {
            if (output == OUTPUT_HEXDUMP)
                hexdump(stdout, "Message Key",
                    rkmessage->key, rkmessage->key_len);
            else
                printf("Key: %.*s\n",
                       (int)rkmessage->key_len, (char *)rkmessage->key);
        }
    
        if (output == OUTPUT_HEXDUMP)
            hexdump(stdout, "Message Payload",
                rkmessage->payload, rkmessage->len);
        else
            printf("%.*s\n",
                   (int)rkmessage->len, (char *)rkmessage->payload);
    }
    
    
    static void print_partition_list (FILE *fp,
                                      const rd_kafka_topic_partition_list_t
                                      *partitions) {
            int i;
            for (i = 0 ; i < partitions->cnt ; i++) {
                    fprintf(stderr, "%s %s [%"PRId32"] offset %"PRId64,
                            i > 0 ? ",":"",
                            partitions->elems[i].topic,
                            partitions->elems[i].partition,
                partitions->elems[i].offset);
            }
            fprintf(stderr, "\n");
    
    }
    static void rebalance_cb (rd_kafka_t *rk,
                              rd_kafka_resp_err_t err,
                  rd_kafka_topic_partition_list_t *partitions,
                              void *opaque) {
    
        fprintf(stderr, "%% Consumer group rebalanced: ");
    
        switch (err)
        {
        case RD_KAFKA_RESP_ERR__ASSIGN_PARTITIONS:
            fprintf(stderr, "assigned:\n");
            print_partition_list(stderr, partitions);
            rd_kafka_assign(rk, partitions);
            wait_eof += partitions->cnt;
            break;
    
        case RD_KAFKA_RESP_ERR__REVOKE_PARTITIONS:
            fprintf(stderr, "revoked:\n");
            print_partition_list(stderr, partitions);
            rd_kafka_assign(rk, NULL);
            wait_eof = 0;
            break;
    
        default:
            fprintf(stderr, "failed: %s\n",
                            rd_kafka_err2str(err));
                    rd_kafka_assign(rk, NULL);
            break;
        }
    }
    
    
    static int describe_groups (rd_kafka_t *rk, const char *group) {
            rd_kafka_resp_err_t err;
            const struct rd_kafka_group_list *grplist;
            int i;
    
            err = rd_kafka_list_groups(rk, group, &grplist, 10000);
    
            if (err) {
                    fprintf(stderr, "%% Failed to acquire group list: %s\n",
                            rd_kafka_err2str(err));
                    return -1;
            }
    
            for (i = 0 ; i < grplist->group_cnt ; i++) {
                    const struct rd_kafka_group_info *gi = &grplist->groups[i];
                    int j;
    
                    printf("Group \"%s\" in state %s on broker %d (%s:%d)\n",
                           gi->group, gi->state,
                           gi->broker.id, gi->broker.host, gi->broker.port);
                    if (gi->err)
                            printf(" Error: %s\n", rd_kafka_err2str(gi->err));
                    printf(" Protocol type \"%s\", protocol \"%s\", "
                           "with %d member(s):\n",
                           gi->protocol_type, gi->protocol, gi->member_cnt);
    
                    for (j = 0 ; j < gi->member_cnt ; j++) {
                            const struct rd_kafka_group_member_info *mi;
                            mi = &gi->members[j];
    
                            printf("  \"%s\", client id \"%s\" on host %s\n",
                                   mi->member_id, mi->client_id, mi->client_host);
                            printf("    metadata: %d bytes\n",
                                   mi->member_metadata_size);
                            printf("    assignment: %d bytes\n",
                                   mi->member_assignment_size);
                    }
                    printf("\n");
            }
    
            if (group && !grplist->group_cnt)
                    fprintf(stderr, "%% No matching group (%s)\n", group);
    
            rd_kafka_group_list_destroy(grplist);
    
            return 0;
    }
    
    
    
    static void sig_usr1 (int sig) {
        rd_kafka_dump(stdout, rk);
    }
    
    int main (int argc, char **argv) {
            char mode = 'C';
        char *brokers = "localhost:9092";
        char *username = "123";
        char *password = "123";
        int opt;
        rd_kafka_conf_t *conf;
        rd_kafka_topic_conf_t *topic_conf;
        char errstr[512];
        const char *debug = NULL;
        int do_conf_dump = 0;
        char tmp[16];
        rd_kafka_resp_err_t err;
        char *group = NULL;
        rd_kafka_topic_partition_list_t *topics;
        int is_subscription;
        int i;
    
        quiet = !isatty(STDIN_FILENO);
    
        /* Kafka configuration */
        conf = rd_kafka_conf_new();
    
            /* Set logger */
            rd_kafka_conf_set_log_cb(conf, logger);
    
        /* Quick termination */
        snprintf(tmp, sizeof(tmp), "%i", SIGIO);
        rd_kafka_conf_set(conf, "internal.termination.signal", tmp, NULL, 0);
    
        /* Topic configuration */
        topic_conf = rd_kafka_topic_conf_new();
    
        while ((opt = getopt(argc, argv, "g:b:u:p:qd:eX:ADO")) != -1) {
            switch (opt) {
            case 'b':
                brokers = optarg;
                break;
            case 'g':
                group = optarg;
                break;
            case 'u':
                username = optarg;
                break;
            case 'p':
                password = optarg;
                break;
            case 'e':
                exit_eof = 1;
                break;
            case 'd':
                debug = optarg;
                break;
            case 'q':
                quiet = 1;
                break;
            case 'A':
                output = OUTPUT_RAW;
                break;
            case 'X':
            {
                char *name, *val;
                rd_kafka_conf_res_t res;
    
                if (!strcmp(optarg, "list") ||
                    !strcmp(optarg, "help")) {
                    rd_kafka_conf_properties_show(stdout);
                    exit(0);
                }
    
                if (!strcmp(optarg, "dump")) {
                    do_conf_dump = 1;
                    continue;
                }
    
                name = optarg;
                if (!(val = strchr(name, '='))) {
                    fprintf(stderr, "%% Expected "
                        "-X property=value, not %s\n", name);
                    exit(1);
                }
    
                *val = '\0';
                val++;
    
                res = RD_KAFKA_CONF_UNKNOWN;
                /* Try "topic." prefixed properties on topic
                 * conf first, and then fall through to global if
                 * it didnt match a topic configuration property. */
                if (!strncmp(name, "topic.", strlen("topic.")))
                    res = rd_kafka_topic_conf_set(topic_conf,
                                      name+
                                      strlen("topic."),
                                      val,
                                      errstr,
                                      sizeof(errstr));
    
                if (res == RD_KAFKA_CONF_UNKNOWN)
                    res = rd_kafka_conf_set(conf, name, val,
                                errstr, sizeof(errstr));
    
                if (res != RD_KAFKA_CONF_OK) {
                    fprintf(stderr, "%% %s\n", errstr);
                    exit(1);
                }
            }
            break;
    
                    case 'D':
                    case 'O':
                            mode = opt;
                            break;
    
            default:
                goto usage;
            }
        }
    
    
        if (do_conf_dump) {
            const char **arr;
            size_t cnt;
            int pass;
    
            for (pass = 0 ; pass < 2 ; pass++) {
                if (pass == 0) {
                    arr = rd_kafka_conf_dump(conf, &cnt);
                    printf("# Global config\n");
                } else {
                    printf("# Topic config\n");
                    arr = rd_kafka_topic_conf_dump(topic_conf,
                                       &cnt);
                }
    
                for (i = 0 ; i < (int)cnt ; i += 2)
                    printf("%s = %s\n",
                           arr[i], arr[i+1]);
    
                printf("\n");
    
                rd_kafka_conf_dump_free(arr, cnt);
            }
    
            exit(0);
        }
    
    
        if (strchr("OC", mode) && optind == argc) {
        usage:
            fprintf(stderr,
                "Usage: %s [options] <topic[:part]> <topic[:part]>..\n"
                "\n"
                "librdkafka version %s (0x%08x)\n"
                "\n"
                " Options:\n"
                "  -g <group>      Consumer group (%s)\n"
                "  -b <brokers>    Broker address (%s)\n"
                "  -u <username>   sasl plain username (%s)\n"
                "  -p <password>   sasl plain password (%s)\n"
                "  -e              Exit consumer when last message\n"
                "                  in partition has been received.\n"
                            "  -D              Describe group.\n"
                            "  -O              Get commmitted offset(s)\n"
                "  -d [facs..]     Enable debugging contexts:\n"
                "                  %s\n"
                "  -q              Be quiet\n"
                "  -A              Raw payload output (consumer)\n"
                "  -X <prop=name> Set arbitrary librdkafka "
                "configuration property\n"
                "               Properties prefixed with \"topic.\" "
                "will be set on topic object.\n"
                "               Use '-X list' to see the full list\n"
                "               of supported properties.\n"
                "\n"
                            "For balanced consumer groups use the 'topic1 topic2..'"
                            " format\n"
                            "and for static assignment use "
                            "'topic1:part1 topic1:part2 topic2:part1..'\n"
                "\n",
                argv[0],
                rd_kafka_version_str(), rd_kafka_version(),
                            group, brokers, username, password,
                RD_KAFKA_DEBUG_CONTEXTS);
            exit(1);
        }
    
    
        signal(SIGINT, stop);
        signal(SIGUSR1, sig_usr1);
    
        if (debug &&
            rd_kafka_conf_set(conf, "debug", debug, errstr, sizeof(errstr)) !=
            RD_KAFKA_CONF_OK) {
            fprintf(stderr, "%% Debug configuration failed: %s: %s\n",
                errstr, debug);
            exit(1);
        }
    
            /*
             * Client/Consumer group
             */
    
            if (strchr("CO", mode)) {
                    /* Consumer groups require a group id */
                    if (!group)
                            group = "rdkafka_consumer_example";
                    if (rd_kafka_conf_set(conf, "group.id", group,
                                          errstr, sizeof(errstr)) !=
                        RD_KAFKA_CONF_OK) {
                            fprintf(stderr, "%% %s\n", errstr);
                            exit(1);
                    }
    
                    /* Consumer groups always use broker based offset storage */
                    if (rd_kafka_topic_conf_set(topic_conf, "offset.store.method",
                                                "broker",
                                                errstr, sizeof(errstr)) !=
                        RD_KAFKA_CONF_OK) {
                            fprintf(stderr, "%% %s\n", errstr);
                            exit(1);
                    }
    
                    /* Set default topic config for pattern-matched topics. */
                    rd_kafka_conf_set_default_topic_conf(conf, topic_conf);
    
                    /* Callback called on partition assignment changes */
                    rd_kafka_conf_set_rebalance_cb(conf, rebalance_cb);
    
                    rd_kafka_conf_set(conf, "enable.partition.eof", "true",
                                      NULL, 0);
    
            }
    
            if (rd_kafka_conf_set(conf, "ssl.ca.location", "ca-cert.pem", errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK
                    || rd_kafka_conf_set(conf, "security.protocol", "sasl_ssl", errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK
                    || rd_kafka_conf_set(conf, "sasl.mechanism", "PLAIN", errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK
                    || rd_kafka_conf_set(conf, "sasl.username", username, errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK
                    || rd_kafka_conf_set(conf, "sasl.password", password, errstr, sizeof(errstr)) != RD_KAFKA_CONF_OK
               ) {
                fprintf(stderr, "%s\n", errstr);
                return -1;
            }
    
            /* Create Kafka handle */
            if (!(rk = rd_kafka_new(RD_KAFKA_CONSUMER, conf,
                                    errstr, sizeof(errstr)))) {
                    fprintf(stderr,
                            "%% Failed to create new consumer: %s\n",
                            errstr);
                    exit(1);
            }
    
            /* Add brokers */
            if (rd_kafka_brokers_add(rk, brokers) == 0) {
                    fprintf(stderr, "%% No valid brokers specified\n");
                    exit(1);
            }
    
    
            if (mode == 'D') {
                    int r;
                    /* Describe groups */
                    r = describe_groups(rk, group);
    
                    rd_kafka_destroy(rk);
                    exit(r == -1 ? 1 : 0);
            }
    
            /* Redirect rd_kafka_poll() to consumer_poll() */
            rd_kafka_poll_set_consumer(rk);
    
            topics = rd_kafka_topic_partition_list_new(argc - optind);
            is_subscription = 1;
            for (i = optind ; i < argc ; i++) {
                    /* Parse "topic[:part] */
                    char *topic = argv[i];
                    char *t;
                    int32_t partition = -1;
    
                    if ((t = strstr(topic, ":"))) {
                            *t = '\0';
                            partition = atoi(t+1);
                            is_subscription = 0; /* is assignment */
                            wait_eof++;
                    }
    
                    rd_kafka_topic_partition_list_add(topics, topic, partition);
            }
    
            if (mode == 'O') {
                    /* Offset query */
    
                    err = rd_kafka_committed(rk, topics, 5000);
                    if (err) {
                            fprintf(stderr, "%% Failed to fetch offsets: %s\n",
                                    rd_kafka_err2str(err));
                            exit(1);
                    }
    
                    for (i = 0 ; i < topics->cnt ; i++) {
                            rd_kafka_topic_partition_t *p = &topics->elems[i];
                            printf("Topic \"%s\" partition %"PRId32,
                                   p->topic, p->partition);
                            if (p->err)
                                    printf(" error %s",
                                           rd_kafka_err2str(p->err));
                            else {
                                    printf(" offset %"PRId64"",
                                           p->offset);
    
                                    if (p->metadata_size)
                                            printf(" (%d bytes of metadata)",
                                                   (int)p->metadata_size);
                            }
                            printf("\n");
                    }
    
                    goto done;
            }
    
    
            if (is_subscription) {
                    fprintf(stderr, "%% Subscribing to %d topics\n", topics->cnt);
    
                    if ((err = rd_kafka_subscribe(rk, topics))) {
                            fprintf(stderr,
                                    "%% Failed to start consuming topics: %s\n",
                                    rd_kafka_err2str(err));
                            exit(1);
                    }
            } else {
                    fprintf(stderr, "%% Assigning %d partitions\n", topics->cnt);
    
                    if ((err = rd_kafka_assign(rk, topics))) {
                            fprintf(stderr,
                                    "%% Failed to assign partitions: %s\n",
                                    rd_kafka_err2str(err));
                    }
            }
    
            while (run) {
                    rd_kafka_message_t *rkmessage;
    
                    rkmessage = rd_kafka_consumer_poll(rk, 1000);
                    if (rkmessage) {
                            msg_consume(rkmessage);
                            rd_kafka_message_destroy(rkmessage);
                    }
            }
    
    done:
            err = rd_kafka_consumer_close(rk);
            if (err)
                    fprintf(stderr, "%% Failed to close consumer: %s\n",
                            rd_kafka_err2str(err));
            else
                    fprintf(stderr, "%% Consumer closed\n");
    
            rd_kafka_topic_partition_list_destroy(topics);
    
            /* Destroy handle */
            rd_kafka_destroy(rk);
    
        /* Let background threads clean up and terminate cleanly. */
        run = 5;
        while (run-- > 0 && rd_kafka_wait_destroyed(1000) == -1)
            printf("Waiting for librdkafka to decommission\n");
        if (run <= 0)
            rd_kafka_dump(stdout, rk);
    
        return 0;
    }
    ```

2.  执行以下命令编译kafka\_consumer.c。

    ```
    gcc -lrdkafka ./kafka_consumer.c -o kafka_consumer
    ```

3.  执行以下命令消费消息。

    ```
    ./kafka_consumer -g <group> -b <bootstrap_servers> -u <username> -p <password> <topic>
    ```

    |参数|描述|
    |--|--|
    |group|Consumer Group名称。您可在消息队列Kafka版控制台的**Consumer Group管理**页面获取。|
    |bootstrap\_servers|SSL接入点。您可在消息队列Kafka版控制台的**实例详情**页面的**基本信息**区域获取。|
    |username|用户名。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的用户名。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。详情请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。 |
    |password|密码。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的密码。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。详情请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。 |
    |topic|Topic名称。您可在消息队列Kafka版控制台的**Topic管理**页面获取。|


