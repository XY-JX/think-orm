# ThinkORM

## 文档

详细参考 [ThinkORM开发指南](https://www.kancloud.cn/manual/think-orm/content)

参考地址 [xy-jx](https://www.yuque.com/xy-jx/xuexi/wnxqqz)
## 安装

~~~
composer require topthink/think-orm
~~~

## 此项目是对ThinkORM的一些修改 【主要增加MongoDB相关操作】

此项目不在跟新，不在维护

## 需要自己手动修改请参考本项目 [ad31721](https://github.com/XY-JX/think-orm/commit/ad3172161349767e0d8229585b1a4a55c31821a4) 版本

~~~
ad3172161349767e0d8229585b1a4a55c31821a4
~~~

## 示例

~~~
 public function mongo1()//分片add
    {
  $money = mt_rand(999, 10000);

        $cs = MCS('cs')->where('_id', '60b5ce62af31000099005545')
            ->save(['list' => ['$push',
                [
                    'cd' => date('Y-m-d H:i:s', TIMESTAMP),
                    'id' => mt_rand(1, 1000),
                    'name' => '测试' . mt_rand(200, 300),
                ],
            ],
                'moneylog' => ['$push', ['createtime' => date('Y-m-d H:i:s'), 'type' => 7, 'brand_id' => '', 'money' => $money, 'moneys' => $money, 'content' => '内容付费:' . $money . '元']]
            ]);

        print_r($cs);
    }
        public function mongo2(){//分片删除

          //  $money = mt_rand(999,10000);

            $cs= MCS('cs')->where('_id','60b5ce88af3100009900554a')
              ->save(['list'=>['$pull', ['id'=>579 ,'cd'=>'2021-06-01 14:18:49']]]);

            print_r($cs);
        }
    public function mongo3(){//elemMatch  分组多条件更新

        //  $money = mt_rand(999,10000);
        $cs= MCS('cs')->where('_id','60b5ce88af3100009900554a')
           ->where('list','elemMatch',['id'=>19,'cd'=>'2021-06-01 15:18:48'])
//       ->where('list.cd','2021-06-01 15:18:48')
//       ->where('list.id',579)

       /*->where('list.id','$elemMatch'=>['id'=>579,'cd'=>'2021-06-01 14:18:48'])*/
            ->save(['list.$.name'=>'就打架大家看看']);

        print_r($cs);
    }
    public function mongo4(){//分片查询

        //  $money = mt_rand(999,10000);
        $cs= MCS('cs')
//            ->limit(10,10)  // limit 和 page 都支持
            ->page(1,10)
            ->where('_id','60b5ce62af31000099005545')
            ->order('list.id','desc')
            ->field('list')
            ->aggregateSelect([['list.id','>',100],['list.name','测试295']]);
        print_r($cs);
    }
  public function mongo5(){//普通统计
      $a = MG('article')->multiAggregate([
            ['sum', 1, 'count'],
            ['sum', 'like'],
            ['sum', 'read'],
        ], ['type'],//  group
        );
 }
 public function mongo6(){//过滤统计
     $show_log = MG('article')->where('originality_id', $id)
       ->multiAggregate([
                        ['sum', 1, 'count'],
                    ],
                        ['space_id', 'day'],//  group
                        ['day' => ['$substr' => ['$cd', 0, 10]], 'space_id' => 1],//project
                    );
 }

~~~






